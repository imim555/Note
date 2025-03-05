
# 언어모델 평가방법

## PPL(perplexity)

- 불확실성을 수치화

- 예측대상의 확률분포가 비슷하다면, 불확실성이 높기 때문에 성능이 낮다고 간주

## Modified Unigram Precision

>[!example]
>- Candidate : the the the the the the the
>- Reference1 : the cat is on the mat
>- Reference2 : there is a cat on the mat

- 중복을 제거하여 보정하기
	- 기계번역을 예측값, 사람번역을 실제값이라고 할 때,
	- 예측값에서 각 단어가 사용된 횟수보다 실제값에서 사용된 횟수가 더 유의미하다고 간주하여
	- 이 두값을 스케일링

- $Count_{clip}\ =\ min(Count,\ Max)$

- $\text{Modified Unigram Precision =}\frac{\sum_{unigram∈Candidate}\ Count_{clip}(unigram)}{\sum_{unigram∈Candidate}\ Count(unigram)}$

## BLUE(Bilingual Evaluation Understudy)


>[!Example]
>
>- Candidate1 : It is a guide to action which ensures that the military always obeys the commands of the party.
>- Candidate2 : It is to insure the troops forever hearing the activity guidebook that party direct.
>- **Candidate3 : the that military a is It guide ensures which to commands the of action obeys always party the.**
>- Reference1 : It is a guide to action that ensures that the military will forever heed Party commands.
>- Reference2 : It is the guiding principle which guarantees the military forces always being under the command of the Party.
>- Reference3 : It is the practical guide for the army always to heed the directions of the party.
-> Ca은 문법에 전혀 맞지 않지만 높은 점수를 받을 수 있음


- 순서정보를 고려하기 위해 유니그램(p1)에서 n-gram(pn)으로 확장

	- $p_{n}=\frac{\sum_{n\text{-}gram∈Candidate}\ Count_{clip}(n\text{-}gram)}{\sum_{n\text{-}gram∈Candidate}\ Count(n\text{-}gram)}$

	- $BLEU = exp(\sum_{n=1}^{N}w_{n}\ \text{log}\ p_{n})$<br><br>

- 문장 길이에 따른 정밀도 문제
	- 예측값의 문장길이가 긴 경우, n-gram 순서정보로 인해 패널티가 적용됨
	- 반면, 예측값의 문장길이가 짧은 경우, 높은 점수를 얻을 수 있기 때문에 별도의 패널티 부여 필요
	- 이를 브레버티 패널티(Brevity Penalty) 라고 한다

	- $BLEU = BP × exp(\sum_{n=1}^{N}w_{n}\ \text{log}\ p_{n})$

	- $BP = \begin{cases}1&\text{if}\space c>r\\ e^{(1-r/c)}&\text{if}\space c \leq r \end{cases}$