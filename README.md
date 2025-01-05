# 👨🏻‍💻 Marketing-Automation Project
- 주 단위로 광고 유입 경로별 성과를 트레킹
- 마케팅 데이터 분석 및 지표 정의

### 🖥️ 분석 기간
Oct, 2024 - Dec, 2024

### 📊 분석 내용
#### 데이터 전처리
- log_data, order_data, campaign_data 전처리
- campaign같은 경우에는 유입경로라고 보면 되는데, 유입경로가 없이 들어오는 경우도 있어서 NaN 값이 있는 경우가 더 많음,
- NaN 값이 있는 경우를 Organic으로 처리
- 테이블을 해석해보면 구매를 한 order_id 1174는 timestamp의 시간에 Campaign_3 경로를 통해서 들어왔다.

#### Week 단위로 유입경로 별 성과 트레킹
- CPC : Cost Per Click -> 캠페인의 클릭 당 얼마를 지불했는지
- ROAS (Return On Advertising Spend) -> 광고 지출 대비 광고를 통해 벌어들인 수익이 얼마인지


        #merging process
        #NaN values = 오가닉 마케팅으로 판단 >> 캠페인을 통해서 들어온 광고가 아님
        #order와 log를 order_id를 기준으로 LEFT JOIN
  
        od1 = pd.merge(order, log, how ='left', left_on = 'order_id', right_on = 'order_id')
        ad_agg = ad.groupby(by = ['campaign', 'week'])[['spend', 'clicks']].sum().reset_index()
        od_agg = od1.groupby(by = ['campaign', 'week'])['order_amount'].sum().reset_index()
        df = pd.merge( od_agg, ad_agg, how = 'left', left_on = ['campaign', 'week'], right_on = ['campaign', 'week'])

  <img width="576" alt="image" src="https://github.com/user-attachments/assets/1c655b99-6328-403b-893f-47d0ec77b0f4" />


### 📈 ROAS = (order_amount)/(df.spend)
<img width="650" alt="image" src="https://github.com/user-attachments/assets/72c7d0e8-5932-4b4f-9f6d-420e6e915361" />
<img width="689" alt="image" src="https://github.com/user-attachments/assets/b649abfa-1be5-4033-a4d3-29493ce3201e" />


- **지표 중요성**
  - Business적으로 ROAS가 커지면 유리함을 나타냄.
  - **높은 ROAS**: 광고 지출 대비 높은 수익을 의미. 광고 캠페인의 효과성 파악.
  - **낮은 ROAS**: 광고 투자에 대한 낮은 수익을 의미. 광고 전략의 개선이 필요함을 시사.
