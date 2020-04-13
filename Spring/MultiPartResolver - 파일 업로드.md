# MultiPartResolver - 파일 업로드
## 환경 설정 : pom.xml
* dependency 추가
```xml
<!-- common fileupload -->
		<dependency>
			<groupId>commons-fileupload</groupId>
			<artifactId>commons-fileupload</artifactId>
			<version>1.2.1</version>
		</dependency>

		<dependency>
			<groupId>commons-io</groupId>
			<artifactId>commons-io</artifactId>
			<version>1.4</version>
		</dependency>
```
## 환경 설정 : spring-servlet.xml
```xml
<!-- 멀티파트 리졸버 -->
	<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		<!-- 최대업로드 가능한 바이트크기 -->
		<property name="maxUploadSize" value="52428800" />
		<!-- 디스크에 임시 파일을 생성하기 전에 메모리에 보관할수있는 최대 바이트 크기 -->
		<!-- property name="maxInMemorySize" value="52428800" / -->
		<!-- defaultEncoding -->
		<property name="defaultEncoding" value="utf-8" />
	</bean>
```
MultiPartResolver는 MultiPart 객체를 컨트롤러에 전달하는 역할을 한다.
## 컨트롤러 작성
```java
import com.douzone.fileupload.service.FileUploadService;

@Controller
public class FileUploadController {

	@Autowired
	private FileUploadService fileUploadService;

	@RequestMapping({"", "/form"})
	public String form() {
		return "form";
	}

	@RequestMapping(value="/upload", method=RequestMethod.POST)
	public String upload(@RequestParam(value="email", required=true, defaultValue="") String email
			, @RequestParam(value="upload-file") MultipartFile multipartFile, Model model) {
		System.out.println("email:" + email);

		String url = fileUploadService.restore(multipartFile);
		model.addAttribute("url", url);

		return "result";
	}

}
```
Controller에서는 MultiPartFile 객체를 통해 파일을 받는다. Controller에서는 컨트롤 역할만 하는 것이 좋기 때문에   
실제 파일을 저장하는 부분은 Service 계층에서 한다.
## 뷰 작성
```jsp
<form method="post" action="${pageContext.request.contextPath }/upload" enctype="multipart/form-data">

	<label>email:</label>
	<input type="text" name="email" value="xxxx@gmail.com">
	<br><br>

	<label>파일1:</label>
	<input type="file" name="upload-file">
	<br><br>

	<br>
	<input type="submit" value="upload">
</form>
```
파일 업로드할 때는 form의 enctype="multipart/form-data"로 작성해야하고 method="post"여야 한다.   
그래야 MultiPartResolver가 MultipartFile 객체를 Controller에 전달할 수 있다.
## 서비스 작성
```java
@Service
public class FileUploadService {
  // 리눅스 기준으로 파일 경로를 작성 (루트 경로인 '/'로 시작한다.)
  // 윈도우라면 workspace의 드라이브를 파악하여 JVM이 알아서 처리해준다.
  // 따라서 workspace가 C드라이브에 있다면 C드라이브에 mysite-uploads 폴더를 생성해 놓아야 한다.
	private static final String SAVE_PATH = "/douzone2020/mysite-uploads";
	private static final String URL = "/image";


	public String restore(MultipartFile multipartFile) {
		String url = "";

		try {
			if(multipartFile.isEmpty()) {
				return null;
			}

			String originFilename = multipartFile.getOriginalFilename();
			String extName = originFilename.substring(originFilename.lastIndexOf('.') + 1);
			String saveFilename = generateSaveFilename(extName);
			long fileSize = multipartFile.getSize();

			System.out.println("######## " + originFilename);
			System.out.println("######## " + saveFilename);
			System.out.println("######## " + fileSize);

			byte[] fileData = multipartFile.getBytes();
			OutputStream os = new FileOutputStream(SAVE_PATH + "/" + saveFilename);
			os.write(fileData);
			os.close();

			url = URL + "/" + saveFilename;

		} catch(IOException ex) {
			throw new RuntimeException("file upload error:" + ex);
		}
		return url;
	}

	private String generateSaveFilename(String extName) {
		String filename = "";

		Calendar calendar = Calendar.getInstance();
		filename += calendar.get(Calendar.YEAR);
		filename += calendar.get(Calendar.MONTH);
		filename += calendar.get(Calendar.DATE);
		filename += calendar.get(Calendar.HOUR);
		filename += calendar.get(Calendar.MINUTE);
		filename += calendar.get(Calendar.SECOND);
		filename += calendar.get(Calendar.MILLISECOND);
		filename += ("." + extName);

		return filename;
	}
}
```
## 문제점과 해결방안
### 1문제) 
FileUploadService의 URL 경로는 "image"로 설정하는 것이 좋다. 정적 파일인 assets/images 폴더가 있기 때문에 파일이 충돌이 일어날 수도 있다. 
### 2문제) 
mysite-uploads 폴더를 보면 파일이 업로드된 것을 확인할 수 있지만 결과 화면을 보면 액박이 뜬다.
즉 실제 파일이 저장된 서버 상의 위치(물리 주소)와 애플리케이션에서 보여주고자 하는 파일 경로(가상 주소)가 일치하지 않아서 생긴 문제이므로
이 부분을 일치시켜주는 작업이 필요하다.
* 환경 설정 : spring-servlet.xml
```xml
<!-- mvc resources -->
	<mvc:resources location="file:/douzone2020/mysite-uploads/" mapping="/image/**">

	</mvc:resources>
```
