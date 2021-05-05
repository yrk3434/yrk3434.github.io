---  
title: CodilityPractice(Lesson)
category: Programming  
order: 2  
---  

Lesson 파트에 있는 문제만 풀었으며, 효율성 등의 문제로 100% 답이 아닌 것도 있다.   
시행착오도 글에 포함했으므로 최종 코드만 보면 된다.   
  
## Lesson1. Iterations  
  
### Binary Gap  
  
```  
def solution(N):  
 # write your code in Python 3.6 bn = format(N, 'b') bn = [int(i) for i in bn] bn_ind = [i for i, e in enumerate(bn) if e ==1] if len(bn_ind)<=1: answer = 0 else: answer = max([i-j-1 for i,j in zip(bn_ind[1:], bn_ind[:-1])]) return answer```  
  
## Lesson2. Array  
  
### Cyclic Rotation  
```  
def solution(A, K):  
 if len(A)==0: answer=[] elif len(A)==1 or len(A)==K: answer=A    elif len(A)<K: # eg. ([1,2,3,4,5],7)  
 move = K%len(A) # 첫번째 element가 이동하는 칸 수  
 start = len(A)-move # 첫 element부터 뒤로 데려갈 elemenet 수 -> 2개 데려가면 3번째부터 elemnt 시작  
 answer = A[start:]+A[:start] elif len(A)>K:  # eg. ([1,2,3,4,5],3) move = K start = len(A)-move answer = A[start:]+A[:start] return(answer)```  
  
### Odd Occurrences In Array  
```  
import collections  
def solution(A):  
 cnt=collections.Counter(A) # input:[4,4,1,1,1,2,2,3] -> Counter({4: 2, 1: 3, 2: 2, 3: 1}) ind=list(cnt.values()).index(1) # 개수 1개인 것  
 return list(cnt.keys())[ind]  
from functools import reduce A=[1,1,6,7,6]  
reduce(lambda x,y: x^y, A)# 바이너리의3은0b11이고5는0b101이므로0b011과0b101의 XOR은0b110이되고 십진수로6이됩니다.  
# reduce(함수, 순서형 자료) 누적적으로(?) 함수에 적용  
# ^: XOR 배타적 OR : eg. 바이너리의3은0b11이고5는0b101이므로0b011과0b101의 XOR은0b110이되고 십진수로6이됩니다. > 다른 자리수 값  
```  
  
재시도, 또 재시도... :(  
  
```  
def solution(A):  
 if len(A)==1: answer = A[0]  
 A = sorted(A) print(A) for i in range(0, len(A), 2): if i+1 == len(A): answer= A[i] if A[i] != A[i+1]: answer = A[i]   
 return answer  
# 위는 안되고 아래는 된다 & 아래도 런타임에러  
def solution(A):  
 if len(A) == 1: return A[0]  
 A = sorted(A) print(A) for i in range(0, len(A), 2): print(i) if i+1 == len(A): return A[i] if A[i] != A[i+1]: return A[i]```  
  
## Lesson3. Time Complexity¶  
  
### FrogJmp  
```  
import math  
def solution(X, Y, D):  
 return math.ceil((Y-X)/D)```  
  
### PermMissingElem  
```  
def solution(A):  
 if len(A)==0:  # empty list input도 꼭 고려하자  
 return 1 if len(A)==max(A): return len(A)+1 return list(set([i for i in range(1,max(A))])-set(A))[0]```  
  
### TapeEquilibrium  
  
```  
# 절반 runtime errorimport itertools  
def solution(A):  
 A_right=list(itertools.accumulate(A)) A_left=[sum(A)-i for i in A_right] diff = [abs(i-j) for i,j in zip(A_right, A_left)] return min(diff)  
# 위 코드는 list를 여러 번 훑어야 함  
def solution(A):  
 if len(A)==1: return A[0] right=0 left=sum(A) diff=[] for i in range(len(A)): right=right+A[i] left=left-A[i] diff.append(abs(right-left)) return min(diff)```  
  
재시도  
```  
# 목적지 도달 못하면 -1인 옵션 추가  
def solution(X, A):  
 if len(A)==0: return -1 elif max(A)<X: return -1 elif len(A)==1 & A[0]>=X: return 0 elif len(A)==1 & A[0]<X: return -1 elif len(A)>1: return [i>=X for i in A].index(True)```  
  
## Lesson4. Counting Elements  
  
### FrogRiverOne  
```  
# 63% 정답 runtime errordef  solution(X, A):  
 if  len(A)==0  or  max(A)<X: return -1  
 all = [i for i in range(1,X+1)] for i in range(len(A)): try: all.remove(A[i]) if len(all)==0: return i except: pass return -1```  
재시도  
```  
# 정확도도 떨어짐  
def solution(X, A):  
 if len(A)<X or max(A)<X: return -1     reach = [i for i,e in enumerate(A) if e>=X]  
 all_leaf =[i for i in range(1,X+1)] for r in reach: if len(set(all_leaf)-set(A[:(r+1)]))==0: return r else: return -1```  
다시 풀어야겠다.  
  
  
### MaxCounters  
  
```  
# 점수0, 다틀림...  
# max 먼저 모아서 계산하고 싶다  
import collections  
def solution(N, A):  
  
 max_ind = [i for i,e in  enumerate(A) if e>N] if len(max_ind)>0: max_ind_old=0 max_add=0 for max_ind_tmp in max_ind: max_add+=max(collections.Counter(A[max_ind_old:max_ind_tmp]).values()) max_ind_old=max_ind_tmp if max_ind[-1]==len(A): return [max_add]*N     NN = [max_add]*N  
 last_add = collections.Counter(A[(max_ind[-1]+1):])    for ind, add in last_add.items():  
 NN[ind-1]=NN[ind-1]+add return NN```  
  
```  
# 너무 어렵게 생각했다. 하지만 이것도 효율성 통과 못함  
def solution(N, A):  
 NN = [0]*N for i in A: if 1<=i<=N: NN[i-1]+=1 else: NN = [max(NN)]*N return NN```  
  
### MissingInteger  
```  
def solution(A):  
 if max(A)<0: return 1 seq = set([i for i in range(1,len(A)+1)]) if len(seq-set(A))==0: return len(A)+1 return min(seq - set(A))```  
  
### PermCheck  
58% 통과  
```  
def solution(A):  
 if len(set([i for i in range(1, len(A))])-set(A))==0: return 1 else: return 0```  
  
83% 통과  
```  
def solution(A):  
 if max(A)!=len(A): return 0 A=list(set(sorted(A))) for i in range(len(A)): if A[i]!=i+1: return 0 else: return 1```  
성공!  
```  
def solution(A):  
 num_A=len(A) if max(A)!=num_A: return 0 # set 추렸을 때 원래 list 길이보다 짧아지면 permutation 아님  
 A=list(set(sorted(A))) if len(A)!=num_A: return 0     for i in range(len(A)):  
 if A[i]!=i+1: return 0 else: return 1```  
  
## Lesson5. Prefix Sums  
  
### CountDiv  
```  
def solution(A, B, K):  
 if A%K==0: # lower bound가 K로 나누어 떨어지는 경우  
 return int(B/K)-int(A/K)+1 else: return int(B/K)-int(A/K)```  
  
### GenomicRangeQuery  
62% 정답, 긴 string에서 효율성 통과 못함  
```  
def solution(S, P, Q):  
 # P: lower index, Q: upper index     dna_dict={'A':1,'C':2,'G':3,'T':4}  
 SS = [i for i in S]  
 # String 1개 or 모두 같은 letter if all([i==SS[0] for i in SS]): return [dna_dict[SS[0]]]*len(P)  
 answer = [] for p,q in zip(P,Q): SS_slice = sorted(SS[p:(q+1)]) answer.append(dna_dict[SS_slice[0]]) # 알파벳 제일 작은 게 다행히 impact 숫자도 작다  
 return answer```  
  
### MinAvgTwoSlice  
50% 정답, 효율성 문제  
```  
# 평균 낼 element 개수 상관 없이 start, end index를 뽑았다.  
import itertools  
  
def solution(A):  
 PQ = list(itertools.combinations(range(len(A)),2)) avg = [] for p,q in PQ: avg.append(sum(A[p:(q+1)])/(q-p+1))  
 min_ind = avg.index(min(avg))  
 return PQ[min_ind][0]```  
  
  
https://cheolhojung.github.io/posts/algorithm/Codility-MinAvgTwoSlice.html <br/>  
이 블로그 설명을 읽고 풀었음 (2개 평균, 3개 평균만 고려하면 되는 이유)  
```  
def solution(A):  
  
 avg1_old = sum(A[0:2])/2 avg2_old = sum(A[0:3])/3 answer = 0     min_old = min(avg1_old, avg2_old)  
     for i in range(1,len(A)-2):  
 avg1 = sum(A[i:(i+2)])/2 avg2 = sum(A[i:(i+3)])/3  
 if min_old>avg1 or min_old>avg2: # 새 평균(avg1, avg2)이 더 작으면 업데이트  
 answer = i min_old = min([avg1, avg2]) avg1_old = avg1 avg2_old = avg2  
 # 마지막 두 개 평균  
 avg1 = sum(A[-2:])/2 if min_old>avg1: answer = i+1     return answer  
```  
  
### PassingCars  
60% 효율성 문제  
```  
def solution(A):  
 # passing car 없는 경우  
 sort_A = sorted(A) if sort_A[0]==sort_A[-1]: return 0  
 # 0인 값 기준 오른쪽 1인 값 더한다  
 passing = 0 passing_list =[] passing_list_sum=0 for i in range(len(A)-2,-1,-1): # 역순, 오른쪽부터 탐색  
 passing += A[i+1] if A[i]==0: passing_list.append(passing) passing_list_sum = sum(passing_list) if passing_list_sum > 1000000000: return -1 return sum(passing_list)```  
  
## Lesson6. Sorting  
### Distinct  
```  
def solution(A):  
 return len(set(A))```  
  
### MaxProductOfThree  
```  
def solution(A):  
 # write your code in Python 3.6 A.sort()  
 a = A[-1] * A[-2] * A[-3] b = A[0] * A[1] * A[-1]     if a > b:  
 return a else: return b```  
  
### NumberOfDiscIntersections  
62% 정답, sort를 안썼더니 효율성 문제가 난다.  
```  
def solution(A):  
 n = len(A) lower = [(i-A[i],-1) for i in range(n)] upper = [(i+A[i],1) for i in range(n)]  
 all_comb = n*(n-1)/2 not_cross =0 for idx, u in enumerate(upper[:-1]): not_cross += sum([1 for l in lower[idx:] if u<l])  
 answer = int(all_comb-not_cross) return -1 if answer>10000000 else answer```  
  
### Triangle  
```  
def solution(A):  
 A = sorted(A)    for i in range(len(A)-2):  
 if A[i+2]<A[i+1]+A[i]: return 1 return ```  
  
## Lesson7. Stacks and Queues  
### Brackets  
다른 블로그 코드를 참고했다. pop을 잘써야하고 조건에 부합하지 않는 걸 빨리 찾으면 조기 return을 해야한다.  
```  
def solution(S):  
 nested = {'(':1, ')':-1, '[':2, ']':-2, '{':3, '}':-3} left=[] for ss in S: if nested[ss]>0: left.append(ss) if nested[ss]<0: # 오른쪽 ) } ] 등장했는데 조기 return 안하면 효율성 문제가 뜬다  
 if len(left)==0: return 0 # 첫 오른쪽 ) } ] 등장하면 대응되는 left 값 옆에 있는지 체크 & pop right = left.pop()            if nested[right] != - nested[ss]:  
 return 0 if len(left)>0: return 0     return 1  
```  
  
### Fish  
내 코드. 통과 못했다.  
```  
def solution(A, B):  
 # index of A & B : fish id # A: size of fish # B: direction, 0(upstream), 1(downstream) # return # of alive fish  
 # 모두 같은 방향  
 if all(map(lambda x: x==0, B)) or all(map(lambda x: x==1, B)):        return len(B)  
  
 for i in range(len(B)-1): try: if B[0]==0: A.pop(0) B.pop(0) if B[-1]==1: A.pop() B.pop() if B[i]!=B[i+1]: if B[i]>B[i+1]: A.pop(i+1) B.pop(i+1) else: A.pop(i) B.pop(i) if all(map(lambda x: x==0, B))  or all(map(lambda x: x==1, B)) \ or len(B)==0:                return  len(B)  
```  
또 다른 블로그 글을 봐버렸다. 에휴  
```  
def solution(A, B):  
 # index of A & B : fish id # A: size of fish # B: direction, 0(upstream), 1(downstream) # return # of alive fish  
 size_down = [] up = 0 for i in range(len(B)): if B[i]==1:            size_down.append(A[i])  
 elif B[i]==0: # upstream 물고기 나타남  
 up += 1 # 내려가는 물고기 중 가장 오른쪽 물고기부터 잡아먹기 게임  
 for j in range(len(size_down)-1,-1,-1):                if size_down[j]>A[i]:  
 up -= 1 # 내려가는 물고기 승  
 break # 중요!! 내려가는 물고기가 이겼으면 if 문 끝내야한다.  
 else: size_down.pop() # 올라가는 물고기 승  
 return up+len(size_down)```  
  
### Nesting  
위 문제 Fish 쉬운 버전이다. 패턴이 적응되니 금방 풀었다.  
```  
def solution(S):  
 # if all(S): #     return 0  
 nested={'(':-1,')':1} left = [] for s in S: if nested[s]<0: left.append(s) if nested[s]>0: if len(left)==0: return 0 left.pop() if len(left)>0: return 0 return 1```  
  
### Stonewall  
전혀 못풀었다..별표 두개! <br/>  
고수가 되어 돌아오면 풀겠다.  
```  
def solution(H):   
    block_cnt = 0  
 stack_arr = []     for i in range(len(H)):  
 # i번째 높이보다 높은 값이 연속되면,  
 # i번째보다 낮은 값 나오는 index 전까지 주춧돌같은 직사각형 깔면됨  
 #        while len(stack_arr) > 0 and stack_arr[-1] > H[i]:  
 stack_arr.pop() # 이전보다 높은 값 나오면 새 블록 필요  
 if len(stack_arr) == 0 or stack_arr[-1] < H[i]: block_cnt += 1 stack_arr.append(H[i])     return block_cnt              
    pass  
```  
cf. 결과 확인 코드  
```  
H=[8, 8, 5, 7, 9, 8, 7, 4, 8]  
  
block_cnt = 0  
stack_arr = []  
  
for i in range(len(H)):  
 print('-------------iter '+str(i)+'-------------') print('current H ='+str(H[i])) while len(stack_arr) > 0 and stack_arr[-1] > H[i]: print('--pop') print(stack_arr.pop())  
 if len(stack_arr) == 0 or stack_arr[-1] < H[i]: block_cnt += 1 stack_arr.append(H[i]) print('--stack_arr') print(stack_arr)```  
  
## Lesson8. Leader  
  
### Dominator  
정답 83%  
```  
# small_half_positions: half elements the same, and half + 1 elements the same  
# extreme_half1: array with exactly N/2 values 1, N even + [0,0,1,1,1]  
import collections  
def solution(A):  
 # occurs in more than half of the elements of A # any! index if len(A)==0: return -1  
 cnt = dict(collections.Counter(A)) dominate = max(cnt.values()) if dominate <= len(A)/2: return -1 d_ind = list(cnt.values()).index(dominate) d_value = list(cnt.keys())[d_ind]     return A.index(d_value)  
 pass  
# solution([0,0,1])  
```  
  
### EquiLeader  
55%, 효율성 X  
```  
import collections  
def solution(A):  
 # occurs in more than half of the elements of A # any! index if len(A)==0: return -1  
 cnt = dict(collections.Counter(A)) dominate = max(cnt.values()) if dominate <= len(A)/2: return -1 d_ind = list(cnt.values()).index(dominate) d_value = list(cnt.keys())[d_ind]     return A.index(d_value)  
 pass```  
  
88% 정답, collections.Counter 연산을 한 번만 하도록 바꿈  
```  
import collections  
def solution(A):  
 left_cnt = collections.Counter(A) # Counter({4: 4, 3: 1, 2: 1}) right_cnt = left_cnt.copy() # Counter({4: 0, 3: 0, 2: 0}) for key in left_cnt.keys(): right_cnt[key]=0     left_n=len(A)  
 right_n=0  
 equi_n=0 for i in range(len(A)): left_n-=1 right_n+=1 left_cnt[A[i]]-=1 right_cnt[A[i]]+=1 # leader 후보의 빈도수가 대상 length의 절반 이상인지,   
        # left equi와 right equi가 같은지  
 if max(left_cnt.values())>left_n/2 and max(right_cnt.values())>right_n/2 and \ max(left_cnt, key=left_cnt.get) == max(right_cnt, key=right_cnt.get):            equi_n+=1  
 return equi_n```
