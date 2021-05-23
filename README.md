# DoBrain Preprocessing

python3
```
conn = pymysql.connect(host='***********', user='********', passwd='**********', db='', port=*******)

pd.read_sql('''SELECT * 
               FROM NEW_DOBRAIN_2020 
               WHERE accountId in {}'''.format(tuple(candidates)), conn)
               
               
```


#### mapping new dobrain data to data scheme of old dobrain
```
from DoBrain.preprocessing import VersionMapper, GameSelector, DragMapper

# To 2019 version style
cols = ['userID', 'level', 'contentIndex', 'questionIndex', 'derivedIndex', 
        'clearDateTime', 'duration','incorrectAnswerCount', 'point']

vm = VersionMapper('./data/두브레인 콘텐츠 MapVersion.xlsx')

drag_new_dobrain = vm.code_mapping(score_new_dobrain, previous_version_style=True, dtype='drag')
score_new_dobrain = vm.code_mapping(score_new_dobrain, previous_version_style=True, dtype='score')


# score
score_new_dobrain['point'] = 2/score_new_dobrain['duration'] + 10/(score_new_dobrain['incorrectAnswerCount'] + 1)
scores = pd.concat([score_old_dobrain[cols], score_new_dobrain[cols]], axis=0)

# Drag
drag_new_dobrain = drag_new_dobrain.loc[~drag_new_dobrain.posX.isna()]
scores = pd.concat([score_new_dobrain, score_old_dobrain], axis=0)
```
