Problem

توقع أقل عمق ملاحي مستقبلي Dmin ستواجهه الباخرة لكل route وdeparture slot وleg، باستخدام معلومات النهر المتاحة حتى issue_time.

Input

بيانات تاريخية hourly لمدة 21 يومًا قبل issue_time، تشمل 21 gauge و16 discharge series، بالإضافة إلى معلومات السيناريو والـroute والـdeparture والـleg.

Output / Labels

أربعة quantiles لـDmin:

q05, q10, q25, q50

Type of ML

Supervised learning + time-series forecasting + quantile regression

Metric

Mean Pinball Loss

Goal

تقليل Pinball Loss، مع التركيز خصوصًا على التنبؤ الصادق بالطرف المنخفض من توزيع Dmin.

Main difficulty

المستقبل بعيد نسبيًا، موجات المياه تتحرك عبر النهر، وDmin تحدده أضعف نقطة في الرحلة.


## Problem Statement
Predict the minimum depth (Dmin) a vessel will encounter for each route, departure slot, and leg using available river information up to issue_time.

## Input
Historical hourly data for 21 days before issue_time, including 21 gauge series and 16 discharge series, plus scenario, route, departure, and leg information.

## Output / Labels
Four quantiles for Dmin:
- q05, q10, q25, q50

## Type of ML
Supervised learning + time-series forecasting + quantile regression

## Metric
Mean Pinball Loss

## Goal
Reduce Pinball Loss, focusing on accurate prediction of the lower tail of Dmin distribution.

## Main Challenge
The future is relatively distant, water waves move through the river, and Dmin is determined by the weakest point along the journey.




لكن لاحظ شيئًا مهمًا:

الـ 504 ساعة نفسها لا يمكننا ببساطة وضعها كلها كـ features.

نحتاج أولًا إلى تلخيصها إلى معلومات مفيدة مثل:

آخر مستوى ماء
متوسط آخر 24 ساعة
minimum آخر 24 ساعة
التغير خلال 6h
التغير خلال 24h
متوسط آخر 7 أيام



## Problem Statement
Predict the minimum depth (Dmin) a vessel will encounter for each route, departure slot, and leg using available river information up to issue_time.

## Input
Historical hourly data for 21 days before issue_time, including 21 gauge series and 16 discharge series, plus scenario, route, departure, and leg information.

## Output / Labels
Four quantiles for Dmin:
- q05, q10, q25, q50

## Type of ML
Supervised learning + time-series forecasting + quantile regression

## Metric
Mean Pinball Loss

## Goal
Reduce Pinball Loss, focusing on accurate prediction of the lower tail of Dmin distribution.

## Main Challenge
The future is relatively distant, water waves move through the river, and Dmin is determined by the weakest point along the journey.




لكن لاحظ شيئًا مهمًا:

الـ 504 ساعة نفسها لا يمكننا ببساطة وضعها كلها كـ features.

نحتاج أولًا إلى تلخيصها إلى معلومات مفيدة مثل:

آخر مستوى ماء
متوسط آخر 24 ساعة
minimum آخر 24 ساعة
التغير خلال 6h
التغير خلال 24h
متوسط آخر 7 أيام
## Problem Statement
Predict the minimum depth (Dmin) a vessel will encounter for each route, departure slot, and leg using available river information up to issue_time.

## Input
Historical hourly data for 21 days before issue_time, including 21 gauge series and 16 discharge series, plus scenario, route, departure, and leg information.

## Output / Labels
Four quantiles for Dmin:
- q05, q10, q25, q50

## Type of ML
Supervised learning + time-series forecasting + quantile regression

## Metric
Mean Pinball Loss

## Goal
Reduce Pinball Loss, focusing on accurate prediction of the lower tail of Dmin distribution.

## Main Challenge
The future is relatively distant, water waves move through the river, and Dmin is determined by the weakest point along the journey.




لكن لاحظ شيئًا مهمًا:

الـ 504 ساعة نفسها لا يمكننا ببساطة وضعها كلها كـ features.

نحتاج أولًا إلى تلخيصها إلى معلومات مفيدة مثل:

آخر مستوى ماء
متوسط آخر 24 ساعة
minimum آخر 24 ساعة
التغير خلال 6h
التغير خلال 24h
متوسط آخر 7 أيام

          
