# Titanic Kaggle
캐글 URL : https://www.kaggle.com/c/titanic<br>
참고한 사이트 : https://www.kaggle.com/ash316/eda-to-prediction-dietanic<br>
작성자 : 김효민<br>
작성날짜 : 2021.02.17

## EDA
* Numpy, Pandas, Matplotlib, Seaborn을 이용
* 주요 Feature<br>
: Survived의 비율과 관련이 있는 주요 Feature <br><br>
    * 성별(Sex), PClass(클래스), Initial(이름 앞에 붙는 이니셜), Embarked(탑승구), Family_Size(Parch+SibSp), Age_band(나이대), Fare_Range(요금대)

    * 성별(Sex) : 여자의 생존률이 남자보다 높음
    * PClass : class가 높은 사람일 수록 생존률이 높음
    * Initial : Mr. , Mrs. 등 이름 앞에 붙는 Initial은 성별과도 연관이 있음으로 Survival 판단에 유용한 Feature가 됨
    * Embarked : S,C,Q 탑승구가 있는데 S의 경우 탑승한 사람이 가장 많으며 남자 탑승객이 많고 Class가 낮은 탑승객 비율이 높은 편으로 생존률이 높지 않은 탑승구. C의 경우 남자와 여자 탑승객의 비율이 비슷하며 높은 클래스의 탑승객의 비율이 높은 편.
    * Family_Size : Parch(부모 수)+SibSp(형제 자매 수)를 합친 데이터로 3,4명의 Family_Size가 생존률이 가장 높음. Family_Size가 너무 작거나 큰 경우 생존률이 낮게 나옴.
    * Age_band : 나이가 적을수록 생존률이 높게 나오는 추세. 나이가 많을수록 생존률이 감소하는 추세.
    * Fare_Range : Fare_Range가 비싼 탑승객일수록 올라가는 생존률

<br><br>
## 결측치 처리

* Age_band 의 경우 각 나이대의 평균값으로 null값을 대체하여 null값의 영향을 최소화

* Embarked의 경우 대부분의 탑승자들이 S 항구에서 탑승하여서 null값은 S 항구로 대체.

* Cabin은 주요 feature로 다루지 않아서 별도로 처리하지 않음

<br><br>
## 기타 Data Processing
* Age, Fare와 같은 continuos한 feature는 나이대처럼 분류 기준을 명확히 함(Age_band, Fare_Range)
* Family_Size의 경우 Parch와 SibSp의 수를 합쳐서 파악할 수 있도록 하였고 별도로 Alone 인지 아닌지 파악하기 위해 Family_Size가 0인 경우 Alone인 것으로 하는 feature를 파악해보았다.

<br><br>
## 사용한 Algorithm 및 앙상블 기법
* 앙상블 : 단일 모델로는 성능의 한계가 있기 때문에 여러 개의 단일 모델들의 평균치를 내거나, 투표를 해서 다수결에 의한 결정을 하는 여러 모델들의 집단 지성을 활용하여 더 나은 결과를 도출하는 것.

    * Voting Classifier : <br>
여러 submodel들의 투표를 통한 결과를 도출. KNN, SVM(Radial Basis Function & Linear), Random Forest, Logistic Regression, Decision Tree, Gaussian NB 조합.

    * Bagging : <br>
    같은 알고리즘 내에서 여러 sample을 조합하는 방식. Bagged-KNN, Bagged Decision Tree.

    * Boosting : <br>
     약한 학습기를 순차적으로 학습하되, 이전 학습에 대하여 잘못 예측된 데이터에 가중치를 부여하여 오차를 보완해나가는 방식.  Adaptive Bossting, Stochastic Gradient Boosting, XGBoost.



<br><br>
## Hyper Parameter Tuning
* Hyper Parameter Tuning의 경우 accuracy값이 가장 높은 2개(저의 경우 SVM, Random Forests)의 classifier의 hyper parameter를 Tuning 해보았다.
* sklearn.model_selection에 있는 GridSearchCV를 이용하여 최적의 hyper-parameter 값을 찾아내는 방식을 이용하였다.