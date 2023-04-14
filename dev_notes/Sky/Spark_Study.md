# Spark Study

1. 데이터 변환: **`select`**, **`withColumn`**, **`drop`**, **`cast`** 등의 함수를 활용하여 데이터 프레임의 컬럼을 선택, 추가, 제거, 형식 변환하는 작업을 해보세요.
2. 필터링 및 조인: **`filter`**, **`where`**, **`join`**, **`left_outer`**, **`right_outer`**, **`inner`**, **`cross`** 등의 함수를 활용하여 데이터 프레임 간의 필터링 및 조인 작업을 진행해보세요.
3. 그룹화 및 집계: **`groupBy`**, **`agg`**, **`sum`**, **`avg`**, **`min`**, **`max`**, **`count`** 등의 함수를 활용하여 데이터를 그룹화하고 집계하는 작업을 진행해보세요.
4. ~~윈도우 함수: **`over`**, **`row_number`**, **`rank`**, **`dense_rank`**, **`lead`**, **`lag`** 등의 윈도우 함수를 활용하여 데이터 프레임에서 윈도우 기반의 연산을 수행해보세요.~~
5. ~~데이터 시각화: **`matplotlib`**, **`seaborn`** 등의 파이썬 시각화 라이브러리와 함께 스파크를 활용하여 데이터를 시각화하는 작업을 진행해보세요.~~

1. 데이터 변환 

|  | spark | pandas |  |
| --- | --- | --- | --- |
| select | df.select(”컬럼명”) | df[’컬럼명’] | 조회 |
| withColumn | df_with_new_column = df.withColumn("city", lit("Seoul")) | df["city"] = "Seoul” | 열 추가 |
| Drop | df_without_age = df.drop("age") | df_without_age = df.drop("age", axis=1) | 열 제거 |
| cast | df = df.withColumn("age", df["age"].cast(IntegerType())) | df["age"] = df["age"].astype(int) | 형 변환 |
1. 필터링 및 조인

|  | spark | pandas |  |
| --- | --- | --- | --- |
| filter | filtered_df = df.filter(df["gender"] == "M") | filtered_df = df[df["gender"] == "M"] | filter와 where은 동일 |
| where | filtered_df = df.where(df["gender"] == "M") | filtered_df = df.query('gender == "M"') |  |
| join | joined_df = df1.join(df2, df1["id"] == df2["id"], "inner") | joined_df = pd.merge(df1, df2, how='inner', on='id')  |  |
1. 그룹화 및 집계

|  | Spark | pandas |  |
| --- | --- | --- | --- |
| groupby | grouped_df = df.groupBy("지역", "성별")  | group_df = df.groupby(["지역", "성별"]).mean().reset_index() | 그룹화 |
| agg | agg_df = grouped_df.agg(avg("월급").alias("평균월급")) | result = group_df.agg({"월급": ["mean", "sum", "min", "max"]}) | 집계 함수 |
| sum | sum_value = df.agg(spark_sum("Value")).collect()[0][0] | sum_value = df["Value"].sum() |  |