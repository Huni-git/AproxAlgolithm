# AproxAlgolithm
## 1. 작업스케줄링
* *__작업 스케줄링__*</br>
최소한의 기계 수로 주어진 작업을 처리하는 문제. 작업의 수행 시간이 중복되지 않도록 모든 작업을 가장 적은 수의 기계에 배정한다. 빠른 시작시간 작업을 우선(Earliest start time first)배정 알고리즘은 최적해를 구할 수 있다.
### 작업 스케줄링 알고리즘 수행과정
![img](https://user-images.githubusercontent.com/80511265/114723064-4f2cc280-9d75-11eb-9ff8-474ac1e6f4e1.png)
![img (1)](https://user-images.githubusercontent.com/80511265/114723092-55bb3a00-9d75-11eb-9802-26291aacd270.png)
![img (3)](https://user-images.githubusercontent.com/80511265/114723118-5ce24800-9d75-11eb-97de-6ca70279351f.png)

### 작업 스케줄링 알고리즘의 응용 분야
비즈니스 프로세싱, 공장 생산 공정, 강의실/세미나룸 배정, 컴퓨터 태스크 스케줄링 등등

**'작업스케줄링(Job Scheduling)** 문제' 는 n개의 작업, 각 작업의 수행 시간 ti(i=1,2,3,4,5,...n) 그리고 m개의 동일한 기계가 주어질 때,</br>
모든 작업이 가장 빨리 종료되도록 작업을 기계(machine)에 배정하는 문제이다.</br>
단, 한 작업은 배정된 기계에서 연속적으로 수행, 기계는 1번에 하나의 작업만을 수행한다.</br> 
작업 스케줄링 문제를 해결하기 위한 가장 간단한 방법은 그리디 알고리즘을 이용해 푸는 것이다.</br>
즉, 배정된 작업이 가장 빨리 끝나는 기계에 새 작업을 배정해주는 것이다. 


## 2. 작업스케줄링 설계과정
작업의 수 n과 각 작업들의 시작 시간과 종료 시간이 기본적으로 입력값으로 들어온다.</br>
또한, 어떠한 경우에도 배치된 기계가 작업의 수보다 많을 수는 없다. 최대 기계 개수는 n값 만큼이 되는 것이다.</br>
우리에게 주어진 정보를 바탕으로 최적해를 구할 수 있는데, 시작시간, 종료시간, 작업의 길이를 바탕으로 알고리즘을 구현해주면 된다.</br>
네 가지 정도의 경우가 존재하는데, 빠른 시작시간 작업 우선, 빠른 종료시간 작업 우선, 짧은 작업 우선, 긴 작업 우선 이렇게가 있다.</br>
이 중에서도 **빠른 시작시간 작업**을 우선한 알고리즘으로 작업스케줄링을 설계하려 했다. 

```java
입력: n개의 작업
출력: 기계에 배정된 작업의 순서

작업 시작 전, 빠른 시작 시간으로 오름차순 정렬하여 배열에 삽입
배열[0] ~ 배열[n-1], i=0 초기화

while(i<n){
if(배열[i] 수행할 수 있는 기계 존재)
 존재시 배열[i]를 해당 기계에 배정
 존재를 안할시 새로운 기계를 찾아 배열[i]를 배정
 i++
}

기계에 배정된 결과 return
```
위와 같은 코드로 작업을 해줄 때, 시간복잡도는 n개 입력받은 작업을 정렬하는 데 O(nlogn) 시간이 걸리고,</br>
입력받은 n을 맞는 기계에 배정해주는 O(m)의 시간, while 루프가 입력만큼 수행되는 시간O(n)을 곱해줘 O(mn)의 시간이 걸린다.</br>
결국, 작업스케줄링에 시간 복잡도는 O(nlogn)+O(mn)이 걸리게 된다.
## 3.자바 코드
```java
public class JobSchedule {
	public static void main(String[] args)
    {
        int n=4,machine=0; // n개의 입력을 받으므로 machine의 개수는 최대 7개까지 가능하다.
        int [] mc=new int[4]; //실행될 기계
        int [][] job= { {2, 5 ,0}, {1, 4, 0}, {6, 9, 0}, {1, 3, 0}};

        for(int i=0;i<n;i++) //시작시간 별로 정렬
        {
            for(int j=0;j<n;j++)
            {
                if(job[i][0]<job[j][0])
                {
                    int index=job[i][0];
                    job[i][0]=job[j][0];
                    job[j][0]=index;
                    index=job[i][1];
                    job[i][1]=job[j][1];
                    job[j][1]=index;
                }
            }
        }
        //정렬된 시간 순서로 작업을 기계에 배치
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(mc[j]<=job[i][0])
                {
                    mc[j]=job[i][1];
                    job[i][2]=j;
                    break;
                }
            }
        }
        //기계 사용 개수 확인해주는 반복문
        for(int i=0;i<n;i++)
        {
            if(mc[i]==0)
            {
                machine=i;
                break;
            }
        }
        
        System.out.println("사용된 기계의 수 : "+machine);
        
        //기계별로 배정된 작업 출력
        for(int i=0;i<machine;i++)
        {
            System.out.print("기계" + (i+1) + " : ");
            for(int j=0;j<n;j++)
            {
                if(job[j][2]==i)System.out.print(job[j][0]+","+job[j][1]+" ");
            }
            System.out.println();
        }
    }

}

```
다음은 입력 4개일 때의 코드 결과이며, 입력의 개수에 따라 입력 시간과 개수를 다르게 해주면 된다.
![제목 없음](https://user-images.githubusercontent.com/80510945/118795749-e2f23100-b8d5-11eb-9ae1-cf5b38241b32.jpg)

~~위 내용은 그리디 알고리즘 과제 내용 중 일부 내용을 수정 본인 깃허브에서 가져와 그대로 작성한 내용이다.~~

## 4. Brute force
지금 까지는 그리디 알고리즘으로 작업스케줄링의 근사해를 구했고 이제는 Bruteforce를 이용해 작업이 기계에 배정될 수 있는 경우의 수 즉 최적해를 구할 것이다. 
Bruteforce코드를 구현하여 기계 2개 중 가장 오래 걸리는 시간을 알아낼 것이다.

우선 만들고자 하는 Brute force의 수도 코드를 알아보자.
![제목 없음](https://user-images.githubusercontent.com/80510945/118797377-7c6e1280-b8d7-11eb-8367-1df56792e59c.jpg)
~~ppt를 참고하여 수도코드를 작성했으며, 위를 바탕으로 프로그램 코드를 짠다.~~
## 5. 자바 코드
```java
import java.util.Random;
public class Approx_Schedulling {
	//x=작업의 개수, y=기계의 개수
	public static int Approximation(int x, int y, int[] t) {
		int [] M= new int[y]; //머신 M이 종료되는 시간.
		//입력의 개수에 따라 M은 2,4,6,8개 등 다양해진다.
		int max_job=0; // 오래 걸리는 기계를 찾아주기 위한 변수
		for(int i=0;i<y;i++) {
			M[i]=0;
		} //모든 M[i]의 값을 초기화 후 작업을 진행.
		for(int i=0;i<x;i++) {
			int min_num=0;
			for(int j=1;j<y;j++) {
				if(M[j]<M[min_num]) { //제일 먼저 끝나는 기계에 작업을 배정하기 위해
					min_num=j; //j가 최소값이 된다.
				}
			}
			M[min_num]=M[min_num]+t[i]; //제일 먼저 끝나는 작업에 시간을 추가해준다.
		}
		for(int i=0;i<y;i++) { //기계 2개 중에 더 시간이 많이 들어간 친구를 찾아주는 역할
			if(M[i]>max_job) {
				max_job=M[i];
			}
		}
		return max_job; // 가장 오래걸린 작업의 시간
	}
	public static void main(String[] args) {
		int n=4;
		int m=2;
		Random random=new Random();
		int [] time= new int[n]; //작업 n개의 수행 시간 
		System.out.print("작업시간:");
		int i;
		for(i=0;i<n;i++) {
			time[i]= random.nextInt(9)+1; //1~9초까지의 작업시간
			if(i==n-1)
				break; //작업시간 마지막은 반복문을 빠져나간 후 작성해준다.
			System.out.print(time[i]+",");
		}
		System.out.println(time[i]);
		System.out.println("두 기계 중 더 많은 시간을 소요하는 작업 시간: "+Approximation(n,m,time));
	}
}
```











