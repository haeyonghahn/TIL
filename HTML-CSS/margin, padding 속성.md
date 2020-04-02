# Margin, Padding 속성
![MarginAndPadding](https://github.com/haeyonghahn/TIL/blob/master/HTML-CSS/images/MarginAndPadding.PNG)

## margin
* margin : Top Right Bottom Left;

1. margin: 10px; -> margin: 10px 10px 10px 10px; (모두 같은 경우)
2. margin: 10px 20px; -> margin 10px 20px 10px 20px; (상하 좌우 같은 경우) 
3. margin: 10px 50px 30px; -> margin: 10px 50px 30px 50px; (좌우만 같은 경우)
> 4. margin: -10px; 라면 ??   
> >![marginPxMinus](https://github.com/haeyonghahn/TIL/blob/master/HTML-CSS/images/marginPxMinus.PNG)
> 5. margin: auto; -> 수평을 중심으로 중앙 정렬을 도와준다.
> > margin: 0 auto;의 개념
> > 상하의 수치는 0, 좌우의 수치는 중앙으로 정렬이라는 뜻과 동일하다.

* margin 속성의 혼합      
더하기 (margin + margin)    
p.a { margin: 30px 0; } /* 가장 큰 값이 적용된다. */   
p.b { margin: 20px 0; }

## padding
margin과 동일한 수치값을 적용하지만 auto 값만 없다고 생각하면 된다.
* padding : Top Right Bottom Left;

1. padding: 10px; -> margin: 10px 10px 10px 10px; (모두 같은 경우)
2. padding: 10px 20px; -> margin 10px 20px 10px 20px; (상하 좌우 같은 경우) 
3. padding: 10px 50px 30px; -> margin: 10px 50px 30px 50px; (좌우만 같은 경우)

* padding 속성의 혼합   
더하기 (padding + padding)   
p.a { padding: 30px 0; } /* 가장 큰 값이 적용된다. */    
p.b { padding: 20px 0; }   
위와 값이 동시에 패딩 값이 적용될 경우 합산되어 30px+20px = 50px가 적용된다.
