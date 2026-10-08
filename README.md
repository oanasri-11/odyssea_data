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


                 PAST
                  │
       ┌──────────┴──────────┐
       │                     │
 Water levels            Discharge
 21 gauges               16 series
       │                     │
       └──────────┬──────────┘
                  │
             issue_time
                  │
                  ▼
             FEATURES X
                  │
                  ▼
              ML MODEL
                  │
                  ▼
       FUTURE Dmin DISTRIBUTION
                  │
       ┌──────────┼──────────┐
       ▼          ▼          ▼
      q05        q10        q25        q50







                 RIVERLOAD C1
                      │
                      ▼
             BARGE IN ROTTERDAM
                      │
             ┌────────┴────────┐
             ▼                 ▼
           WAAL               LEK
             │                 │
             └────────┬────────┘
                      │
              Departure Slot
                 0 ... 24
                      │
                      ▼
                    Leg
                      │
                      ▼
               FUTURE JOURNEY
                      │
        ┌─────────────┴─────────────┐
        │                           │
   River segments              River segments
        │                           │
        ▼                           ▼
     Depth 1                     Depth 1
     Depth 2                     Depth 2
       ...                         ...
        │                           │
        └─────────────┬─────────────┘
                      ▼
                    Dmin
             = minimum depth
                      │
                      ▼
            ┌─────────┼─────────┐
            ▼         ▼         ▼
           q05       q10       q25       q50