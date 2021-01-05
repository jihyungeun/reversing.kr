![image](https://user-images.githubusercontent.com/76879581/103632843-45cad780-4f88-11eb-972b-d13854d6d6c5.png)  
흔히보이는 입력 프로그램이다.  
![image](https://user-images.githubusercontent.com/76879581/103632850-482d3180-4f88-11eb-8b78-1bace5f0244e.png)  
아무값이나 입력해보니 "Incorrect Password" 문자열이 나온다.  
알맞은 패스워드를 구해줘야하는 문제로 보인다.  
![image](https://user-images.githubusercontent.com/76879581/103632863-4c594f00-4f88-11eb-948c-0b94637676e1.png)  
win32 프로그램이다.  
X32dbg로 분석했다.  
![image](https://user-images.githubusercontent.com/76879581/103632868-4e231280-4f88-11eb-9de0-4ba4ab50c90d.png)  
우선 프로그램에서 패스워드가 틀렸을때 "Incorrect Password"라는 문자열을  출력한다는 사실을 알고있다.  
![image](https://user-images.githubusercontent.com/76879581/103632876-51b69980-4f88-11eb-94bb-293a98573ad8.png)  
Incorrect Password 문자열이 보인다.  
그위쪽에는 맞는 패스워드를 구할 시 출력될거로 보이는 "Congratulation !!"문자열도 보인다.  
맞는 패스워드를 구하는게 목표이니 "Congratulation" 문자열이 있는 위치로 이동해주었다.  
![image](https://user-images.githubusercontent.com/76879581/103632884-5418f380-4f88-11eb-8207-5f51eafc7295.png)  
Congratulation 문자열이 출력되기까지 생각보다 많은 분기점이 있다.  
일단 문자열참조들을 보면 'a', 'R3versing', 'E'들이 눈에보인다.  
![image](https://user-images.githubusercontent.com/76879581/103632901-5713e400-4f88-11eb-9cf1-d4dbfce514b1.png)  
일단 눈에보이는 문자들을 조합해서 실행해보았다.  
![image](https://user-images.githubusercontent.com/76879581/103632908-58451100-4f88-11eb-823f-8f262b818003.png)  
ecx에 R3versing이라는 문자열이 담겼다.  
![image](https://user-images.githubusercontent.com/76879581/103632920-5a0ed480-4f88-11eb-9b64-d21fe871e248.png)  
하지만 test eax, eax를 통해 eax에 1로 세팅되며 그대로 실패문자열로 이동하게 되었다.  
입력한 문자열은 "EaR3versing"이고 ecx에는 R3versing이 담겼다.  
그리고난뒤 easy_crackme.401150 함수를 호출하게 된다.  
아무래도 저 함수에서 어떤 동작을 하는지 알아야할 것 같다.  
![image](https://user-images.githubusercontent.com/76879581/103632929-5da25b80-4f88-11eb-8984-f131d7ff94b5.png)  
내가 입력한 문자열에서 ecx로 가져가는지, 아니면 이미 있는 "R3versing"문자열을 그냥 가져오는지 궁금해서  
문자열을 아무렇게나 바꾸고 다시 실행해보았지만 jne에 바로 걸리며 실패문자열로 이동하게 된다.  
![image](https://user-images.githubusercontent.com/76879581/103632935-6004b580-4f88-11eb-9e2c-ebafce4e3762.png)  
바로위에 있는 cmp에서 걸린거같은데 esp+5에 "BCDEFG"가 있고, 'B'가 입력되었기에 통과가 안된모양이다.  
이로써 2번째 문자열이 'a'여야만 통과가 되나보다. 문자열을 AaCDEFG로 바꾸겠다. 
![image](https://user-images.githubusercontent.com/76879581/103632937-6135e280-4f88-11eb-9b35-3552326f315c.png)  
역시나 그냥 통과된다.  
이로써 패스워드의 2번째 문자는 'a'인걸로 확정되었다.  
계속 분석해보자.  
![image](https://user-images.githubusercontent.com/76879581/103632942-63983c80-4f88-11eb-81ad-0b0ee308de48.png)  
ecx에 "CDEFG"가 저장된다.  
ecx에는 아마 3번째 문자부터 받아오는것 같다.  
![image](https://user-images.githubusercontent.com/76879581/103632946-65620000-4f88-11eb-9581-fff52e04f9f1.png)  
이번에는 ecx에 3번째문자열부터 가져온뒤 어떤 동작을 수행하는지 알기위해  
호출된 함수로 이동해주었다.  
![image](https://user-images.githubusercontent.com/76879581/103632951-67c45a00-4f88-11eb-8b32-daa3e47bd164.png)  
우선 함수내에서 xor연산을 진행한후  
"C"와 "5"를 비교한다.  
3번째 문자는 5라는걸 알려주는건지.. 아직은 모르겠다.  
![image](https://user-images.githubusercontent.com/76879581/103632955-698e1d80-4f88-11eb-852c-d07974ef8eb7.png)  
이대로 함수를 나오게되면 그대로 jne에 걸리게 된다.  
이번엔 "Aa5DEFG"로 입력해보고 실행해봐야겠다.  
![image](https://user-images.githubusercontent.com/76879581/103632963-6abf4a80-4f88-11eb-96b5-2d2670ba88ce.png)  
하지만 또 jne에 걸리게된다.  
Easy_crackme.401150 이나 easy_crackme.401135함수에 뭐가있는지 다시 살펴봐야겠다.  
![image](https://user-images.githubusercontent.com/76879581/103632966-6c890e00-4f88-11eb-959b-e3fabd6e40ed.png)  
연산하는걸 다시보니 여기서 eax가 xor연산을 통해 '7'의 값을 가지게된다.  
내가 입력한 문자열이 "Aa5DEFG"니 7글자의 문자를 넣어줬다.  
아마 해당연산을 통해 문자열의 길이를 유추하는 것 같다.  
![image](https://user-images.githubusercontent.com/76879581/103632974-6e52d180-4f88-11eb-8891-73de25ed0860.png)  
여기서 왜그런지는 모르겠는데 edi에는 "EFG", esi에는 "5DEFG"를 넣어준다.  
![image](https://user-images.githubusercontent.com/76879581/103632979-6f83fe80-4f88-11eb-8434-7b73a619ee13.png)  
그리고 여길보면 'y'가 있는지 없는지를 검사한다.  
'D'에 'y'가있는지 검사하는걸 보니 4번째 문자는 'y'인가보다.  
그러면 이번엔 "Aa5yEFG"로 입력해봐야겠다.  
![image](https://user-images.githubusercontent.com/76879581/103632982-714dc200-4f88-11eb-9378-bdcee6dfdf10.png)  
이번엔 저 jne가 통과되었다.  
이제 계속 분석해보자.  
![image](https://user-images.githubusercontent.com/76879581/103632986-73178580-4f88-11eb-8250-63a87e10673e.png)  
'E'와 'R'을 비교한다. 아무래도 5번째부턴 "R3versing" 저 문자열이 들어가야하나 보다.  
![image](https://user-images.githubusercontent.com/76879581/103632994-74e14900-4f88-11eb-89aa-6e7db9d0d178.png)  
이렇게 입력해서 진행해보겠다.  
![image](https://user-images.githubusercontent.com/76879581/103633002-7743a300-4f88-11eb-99ea-e54ab3cbeb25.png)  
보아하니 저기서 계속 반복을하며 한글자씩 R3versing이 입력됐는지 확인하는 것 같다.  
![image](https://user-images.githubusercontent.com/76879581/103633010-790d6680-4f88-11eb-9bca-c187d6773fc0.png)  
역시 한글자씩 지워가며 문자열을 제대로 입력했는지 비교한다.  
![image](https://user-images.githubusercontent.com/76879581/103633019-7ad72a00-4f88-11eb-84d8-a98195ada344.png)  
여기서 'A'와 'E'를 비교한다.  
첫글자가 'E'여야하나 보다.  
"Ea5yR3versing"으로 입력하고 다시 실행해주겠다.  
![image](https://user-images.githubusercontent.com/76879581/103633027-7d398400-4f88-11eb-8b24-93e31e50107c.png)  
풀렸다.