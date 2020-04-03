# inline-block, block, inline   
### { display:inline; }  
대표적으로 <span>이라는 태그의 성질로 content/text 크기만큼만 점유하고
동일 라인에 붙는 성질입니다.
- width/height 적용 불가
- margin/padding-top/bottom 적용 불가
- line-height 원하는 대로 적용 불가(span에 적용안되고 감싸고 있는 div 전체 크기에만 영향 등)   
### { display: block; }  
반면 block은 무조건 한줄을 점유하고, 다음 태그는 다음 줄로 가버린다.
### { display: inline-block; }   
- width/height 적용 가능   
- margin/padding-top/bottom 적용 가능   
- line-height 적용 가능   
다만 고려해야 할 것이 있다. inline-block 끼리 공백이 생기게 되는데, 이때는 상위 div에 { font-size: 0; } 를 적용하면 해결이 됩니다. inline-block 끼리 높이가 안맞을시 상위 공백이 생기는데, 이때는 { vertical-align: ---; } 값으로 top 등을 줘서 맞춰주시면 됩니다.

