# <Project 1. 자유전공학부 학생들의 이동경로 분석을 통한 강의동 재배치 연구>
Link : https://github.com/Won-Ryeol/Building_Optimization_SNU

Abstract
- 학생들의 캠퍼스 내 이동 동선이 비효율적인 것을 인식.
- 많은 학생들이 공통적으로 겪고 있는 불편함임을 확인.
- 학내 각 강의동의 중요도를 측정하여 최적의 강의동 배치를 탐색.

Data Extract
- 가능한 모집단을 대표하는 표본을 위해, 전공이 다양한 자유전공학부 학생들의 시간표 정보를 수집. - 총 120개 이상의 시간표 데이터를 제공받음.

Data Transform
- 각 강의동을 node, 각 edge를 하나의 동선으로 간주.
- Node는 건물의 단과대 정보를 포함, edge는 각 동선을 수행하는 학생의 id, 계열 정보, 강의간 공강 시간을 포함.
- 시간표 정보를 통해 강의동 간 전체 이동 동선을 directed graph로 저장(34 nodes, 764 edges)

Data Analysis
- 각 node의 GPS 정보를 이용하여 실제 지도 위에 네트워크를 visualization.
- 각 edge의 weight를 부여 : 강의 간 공강 시간이 짧을수록 더 가까이 위치해야 함.
  ※ Weight = 빈도수*100/(공강 시간)
- 각 Node 사이의 거리는 이에 대응하는 edge의 weight에 비례하도록 Node들을 지도상에 재배치. (by Fruchterman Rinegold layout in Gephi)
- 위의 과정을 통해 얻은 건물 배치도에 따라 강의를 배정할 경우, 학생들의 이동 동선이 최소화 될 것.



# <Project 2. 지하철 플랫폼 혼잡도 예측>
Link : https://github.com/Won-Ryeol/subway-conjestion-analysis

Abstract
- 서울지하철은 이용량과 영업 거리로 보았을 때 모두 세계에서 열 손가락 안에 꼽힐 정도로 많은 사람들이 이용하고 있다. 따라서, 특정 시간에 역사 내 혼잡도를 미리 예측하는 것이 필수적.
- 과거 승객들의 승하차 데이터와 기계 학습 기법(MLP, RF)을 이용해 특정 시간에 특정 역의 혼잡도를 예측.

Problem definition
- 혼잡도 = 승객 수 / 수용 인원 * 100 (%)로 정의 가능
- 이때, 평균 승객 수 = K*(시간 당 승객 수)*(역사 내에 머문 시간)    (단, K는 상수)
- 승객은 승차 인원과 하차 인원으로 분류. 승차 인원은 승차 전 플랫폼에 머무는 시간이 존재, 하차 인원은 그렇지 않음.
  ※ T_승차 = (배차 간격)/2 + (역사 진입 시간), T_하차 = (역사 진입 시간)
- 평균 배차 간격, 역사 진입 시간, 역 내 수용 인원을 고려하여, 다음과 같은 수치를 도출함.
  ※ 혼잡도 = (0.75*승차 인원 + 0.25*하차 인원) / 4800

Data Extract / Transform
- 2008년~2019년까지 약 87만개의 서울 지하철 1호선 일별 시간별 승/하차 승객수(by 서울시 공공데이터)
- Raw Data를 월, 요일(평일/토요일/일요일), 역명, 시간, 혼잡도 5개의 column으로 가공.

Training
- MLP, RF 기반의 supervised learning 시행.
- 혼잡도를 label로 하여, 다른 4개의 parameter에 대해 학습.
- Training set : 2008~2018, Validation set : 2019

Conclusion
- Validation 결과, 87% 이상의 accuracy로 혼잡도 예측 가능.



# <Project 3. 리그오브레전드 정글러 갱킹 선호도 분석>
Link : https://github.com/Won-Ryeol/LOL_jungler_analysis/blob/master/Jungle_User_Analysis.ipynb

Abstract
- 기존 리그오브레전드 데이터 분석 서비스에서 잘 사용되지 않던 인게임 map data를 활용.
- 맵 전체에서 활동하는 정글러 포지션의 행동을 분석.
- 상대편 정글러의 각 라인 개입 정보를 수집. 시간대별 빈도 분석을 통해 상대에 대한 대처 가능.

Data Extract / Transform
- Riot API를 통해 2분 당 최대 100개의 data GET request 가능.
- 타겟 유저의 최근 100여 경기의 meta data / timeline data를 얻고, 그 중 ‘강타’ 주문을 사용한 경기, 정글 몬스터 처치수가 일정 이상인 경기만을 필터링.(진영에 따라 Blue/Red로 구분)
- 경기 시작부터 10분까지 1분 단위로, 유저의 위치를 추적.(time interval 사이에 처치 관여가 있을 경우, 해당 위치도 포함)
- 추적한 점들의 sequence를 경기 별, 시간 별로 저장

Data Analysis
- 각 시간대, 진영 별 해당 유저의 위치를 mini map 상에 scatter plot, heatmap 형태로 나타냄.
- 각 라인의 위치를 특정한 뒤, 상대 정글러가 시간대별로 라인에 개입한 횟수를 로깅.
- 최근 경기에 근거하여 시간대별 갱킹 선호도를 그래프로 나타냄
