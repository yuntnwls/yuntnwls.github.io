---
title : "[C#] 람다식(Lambda Expression) 사용"
categories: 
    - Study
tag : 
    - C#
    - lambda
toc : true
use_math : false
---

#### [C#] 람다식(Lambda Expression)에 대한 의문점


#### 람다식
람다식은 무명 메서드와 비슷하게 무명 함수를 표현하는데 사용한다.
C# 3.0부터 지원을 시작하였으며 '=>' 연산자를 사용한다.
간단한 사용 예시를 정리해보자.
먼저 람다식을 사용하지 않은 함수는 아래와 같다.

    private int Square(int num){
        return num * num;
    }

이 함수가 람다식으로 정의되면 이와 같이 변경될 수 있다.

    private int LamdaSqare(int num) => num * num;


#### 람다식에 대한 생각
람다식 코드를 보면서 편리하면서도 몇가지 의문점이 들었다.
1. 가독성
람다식을 사용하는 이유 중 가장 큰 이유는 코드의 간결성으로 가독성을 높이는 것이라 이해했다. 하지만 너무 많은 람다식을 적용한 코드는 오히려 가독성을 해치고 있다고 생각되었다. 물론 예시와 같이 간결한 코드의 경우 한눈에 표현이 가능하기에 코드의 가독성이 높아진다는 의견에 찬성했다.

2. 디버깅
다음으로 람다식의 디버깅에 대한 의문이였다.
처음 람다식을 접했을 때 이 코드는 어떻게 디버깅을 해야할지 많이 고민되었다. 다음 라인으로 넘어가면 바로 끝나버리고, 그 내부로 들어가는 방법도 실행되지 않았기에 저 람다식 도중에 하나라도 Exception이 발생하는 부분을 찾아내기 어렵지 않을까 생각했다. 이 의문은 좀 더 람다식을 사용해보면서 어떤 방식으로 디버깅하는게 좋을지 알아볼 예정이다.

3. 성능
마지막으로 람다식의 성능에 대한 의문이다.
코딩을 하다가도 람다식으로 쓰인 부분이 주기적으로 자주 불리는 곳이라면 성능이 기존과 동일할까? 생각하다가 직접 테스트 해보기로 했다.
아래와 같이 일반 함수의 퍼포먼스와 람다식 함수의 퍼포먼스를 비교해보았다.
 	    
        static void Main(string[] args)
        {
            int sum = 0;
            int count = 100000;
            Stopwatch sw = new Stopwatch();
            Console.WriteLine($"## Repeat Count = {count}");
            
            sw.Start();
            for (int i = 0; i < count; i++)
            {
                sum += Square(i);
            }
            sw.Stop();
            Console.WriteLine($"Nomal Sum = {sum}");
            Console.WriteLine($"Nomal Time(tick) = {sw.ElapsedTicks}");
            
            // 1 tick = 100ns
            sum = 0;
            sw.Start();
            for (int i = 0; i < count; i++)
            {
                sum += LamdaSqare(i);
            }
            sw.Stop();
            Console.WriteLine($"Lamda Sum = {sum}");
            Console.WriteLine($"Lamda TIme(tick) = {sw.ElapsedTicks}");
            Console.ReadLine();
        }
            
        // 일반 함수
        private static int Square(int num)
        {
            return num * num;
        }

        // 람다식 적용 함수
        private static int LamdaSqare(int num) => num * num;

예시를 실행하여 테스트 해본 결과 

<img src="/assets/images/study/lambda_performance.png" width="30%" height="" title="퍼포먼스 측정 결과" alt="LambdaPerformance"/>

일반식 : 9,288tick = 928,800ns
람다식 : 12,683tick = 1,268,300ns
=> 약 1.3배 정도로 더 많은 시간이 소요되었다.

물론 테스트를 할 때마다 약간의 차이는 있었으나 대력 1.3배로 평균이 구해졌다. 이렇게 테스트해본 결과 람다식이 성능적으로 좋지 않다는 것을 알게 되었다.

#### 결론
아직 람다식을 많이 접해보지 않은 단계라서 섣불리 판단할 수는 없으나 이러한 성능과 디버깅 이슈는 고려하면서 적용해야겠다.
특히 성능에 관해서는 주기적으로 자주 불리는 함수의 경우 람다식의 표현을 사용하지 않고 구현할 것이며, UI에 사용자에게 제공되는 이벤트나 자주 불리지 않는 코드 정도에서 적용할 것이다.
앞으로 더 알게되거나 의문점이 생기면 계속 해당 포스팅을 수정하면서 이해하도록 노력하자! 