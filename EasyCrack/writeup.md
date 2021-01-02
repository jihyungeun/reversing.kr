![image](https://user-images.githubusercontent.com/76879581/103462949-2eaf9e00-4d6c-11eb-8a61-b6a023eda1b0.png)  
흔히 보이는 입력 프로그램이다.  
![image](https://user-images.githubusercontent.com/76879581/103463014-7f26fb80-4d6c-11eb-9699-4f6fd5500e8c.png)  
아무값이나 입력해보니 "Incorrect Password" 문자열이 나온다.  
알맞은 패스워드를 구해줘야하는 문제로 보인다.  
![image](https://user-images.githubusercontent.com/76879581/103463070-083e3280-4d6d-11eb-9a02-f5abf697c088.png)  
32bit 프로그램이니 x32dbg로 분석했다.  
![image](https://user-images.githubusercontent.com/76879581/103463078-1e4bf300-4d6d-11eb-9a01-7291cdf76dd4.png)  
우선 프로그램에서 패스워드가 틀렸을 때 "Incorrect Password"라는 문자열이  
출력된다는 사실을 알고 있기에 디버거 내에서 문자열 검색으로  
"Incorrect Password"를 찾아줬다.  
![image](https://user-images.githubusercontent.com/76879581/103463106-476c8380-4d6d-11eb-93d9-570ce439cbf8.png)  
"Incorrect Password" 문자열이 보인다.  
그 위쪽에는 맞는 패스워드를 구할 시 출력되는걸로 보이는 "Congratulation !!" 문자열도 보인다.  
맞는 패스워드를 구하는게 목적이니 "Congratulation" 문자열이 있는 위치로 이동해주었다.  
![image](https://user-images.githubusercontent.com/76879581/103463140-84387a80-4d6d-11eb-875c-62d847239419.png)  
"Congratulation" 문자열이 출력되기까지 생각보다 많은 분기점이 있다.  
일단 문자열 참조들을 보면 'a', 'R3versing', 'E'들이 눈에보인다.  
![image](https://user-images.githubusercontent.com/76879581/103463154-a5996680-4d6d-11eb-81ac-f05e4fde4157.png)   
일단 눈에보이는 문자들을 조합해서 실행해보았다.  
![image](https://user-images.githubusercontent.com/76879581/103463865-ee075300-4d72-11eb-80dc-f49df9cea36b.png)  
ecx에 R3versing이라는 문자열이 담겼다.  
![image](https://user-images.githubusercontent.com/76879581/103463940-77b72080-4d73-11eb-8621-f7c14529f6f8.png)  
하지만 test eax, eax를 통해 eax가 1로 세팅되며 그대로 실패문자열로 이동하게 되었다.  
입력한 문자열은 "EaR3versing"이고 ecx에는 "R3versing"이 담겼다.  
그리고난뒤 easy_crackme.401150함수를 호출하게 된다.  
아무래도 저 함수에서 어떤 동작을 하는지 알아야할 것 같다.  
![image](https://user-images.githubusercontent.com/76879581/103463970-ae8d3680-4d73-11eb-94fc-180be205bc09.png)  
내가 입력한 문자열에서 ecx로 가져가는지, 아니면 이미 있는 "R3versing"문자열을  
그냥 가져오는건지 궁금해서 문자열을 아무렇게나 바꾸고 다시 실행해보았다.  
하지만, jne에 바로 걸리며 실패문자열로 이동하게 되었다.  
![image](https://user-images.githubusercontent.com/76879581/103463991-ded4d500-4d73-11eb-99ca-31a06056f77a.png)  
바로 위에있는 cmp에서 걸린거 같은데 esp+5에 "BCDEFG"가 있고,  
'B'가 입력되었기에 통과가 안된 모양이다.  
이로써 올바른 패스워드의 2번째 문자는 'a'임을 알 수 있었다.  
패스워드를 "AaCDEFG"로 바꾸고 다시 분석을 시도했다.  
![image](https://user-images.githubusercontent.com/76879581/103464009-0e83dd00-4d74-11eb-92fe-e867f876f52a.png)  
역시나 그냥 통과된다.  
이로써 패스워드의 2번째 문자는 'a'인걸로 확정되었다.  
계속 분석해보자.  
![image](https://user-images.githubusercontent.com/76879581/103464028-29eee800-4d74-11eb-9230-563cc64e57df.png)  
ecx에 "CDEFG"가 저장된다.  
ecx에는 아마 3번째 문자부터 받아오는것 같다.
![image](https://user-images.githubusercontent.com/76879581/103464038-40953f00-4d74-11eb-9e57-969c4120a87b.png)  
이번에는 ecx에 3번째 문자열부터 가져온뒤  
어떤 동작을 수행하는지 알기위해 호출된 함수로 이동해주었다.  
![image](https://user-images.githubusercontent.com/76879581/103464056-5dca0d80-4d74-11eb-81b0-4a77413293cc.png)  
우선 함수내에서 xor연산을 진행한 후 'C'와 '5'를 비교한다.  
3번째 문자는 5라는걸 알려주는건지.. 아직은 모르겠다.  
![image](https://user-images.githubusercontent.com/76879581/103464193-198b3d00-4d75-11eb-99c9-d4d49ded9481.png)  
이대로 함수를 나오게되면 그대로 jne에 걸리게된다.  
이번엔 "Aa5DEFG"로 입력해보고 실행해봐야겠다.  
![image](https://user-images.githubusercontent.com/76879581/103464208-37f13880-4d75-11eb-87fd-f7799c2af1c4.png)  
하지만 또 jne에 걸리게된다.  
easy_crackmek.401150 이나 easy_crackme.401135 함수에 뭐가 있는지 다시 살펴봐야겠다.  
![image](https://user-images.githubusercontent.com/76879581/103464219-59eabb00-4d75-11eb-8a36-499882b5fb51.png)  
연산하는걸 다시보니 여기서 eax가 xor연산을 통해 '7'의 값을 가지게 된다.  
내가 입력한 문자열이 "Aa5DEFG"니 7글자의 문자를 넣어줬다.  
아마 해당연산을 통해 문자열의 길이를 유추하는 것 같다.  
![image](https://user-images.githubusercontent.com/76879581/103464237-8272b500-4d75-11eb-91bb-1250b292076a.png)  
여기서 왜그런지는 잘 모르겠는데 edi에는 "EFG", esi에는 "5DEFG"를 넣어준다.  
![image](https://user-images.githubusercontent.com/76879581/103464248-9ae2cf80-4d75-11eb-8ae8-abfff4db7541.png)  
그리고 여길보면 'y'가 있는지 없는지를 검사한다.  
'D'에 'y'가 있느니 검사하는걸 보니 4번째 문자는 'y'인가보다.  
그러면 이번엔 "Aa5yEFG"로 입력해봐야겠다.  
![image](https://user-images.githubusercontent.com/76879581/103464267-b5b54400-4d75-11eb-85b7-e2240b82dd68.png)  
이번엔 저 jne가 통과되었다.  
이제 계속 분석해보자.  
![image](https://user-images.githubusercontent.com/76879581/103464278-c5cd2380-4d75-11eb-8d40-6bc60fb1fa1f.png)  
'E'와 'R'을 비교한다. 아무래도 5번째부턴 "R3versing" 저 문자열이 들어가야하나 보다.  
![image](https://user-images.githubusercontent.com/76879581/103464315-062ca180-4d76-11eb-9638-2fdfc048c31c.png)  
이렇게 입력해서 진행해보겠다.  
![image](https://user-images.githubusercontent.com/76879581/103464323-12186380-4d76-11eb-80e6-e5c88519206f.png)  
보아하니 저기서 계속 반복을하며 한글자씩 R3versing이 입력됐는지 확인하는 것 같다.  
![image](https://user-images.githubusercontent.com/76879581/103464335-29575100-4d76-11eb-818e-9353f30c276e.png)  
역시 한글자씩 지워가며 문자열을 제대로 입력했는지 비교한다.  
![image](https://user-images.githubusercontent.com/76879581/103464343-3a07c700-4d76-11eb-8037-a495ca79fab9.png)  
여기서 'A'와 'E'를 비교한다.  
첫글자가 'E'여야하나 보다.  
"Ea5yR3versing"으로 입력하고 다시 실행해주었다.  
![image](https://user-images.githubusercontent.com/76879581/103464350-560b6880-4d76-11eb-8caa-f5e7a4b8ac09.png)  
풀렸다.