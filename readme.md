## step 1 : get data

```
from google.colab import drive
drive.mount("/content/drive")
```
Mounted at /content/drive
```
file_name = "/content/kaggle.json"
with open(file_name, 'r') as f:
    document =  js.loads(f.read())
    
print(document)
#{'key': 'c00046e4b093c67705ca6d402f00aa31', 'username': 'nguynnguynkhoa'}

os.environ['KAGGLE_USERNAME'] = document['username']
os.environ['KAGGLE_KEY'] = document['key']

#get API
```
{'username': 'nguynnguynkhoa', 'key': '86d54991913d6b87880d297405c8173e'}
```
!kaggle datasets download -d uciml/adult-census-income
```
Downloading adult-census-income.zip to /content
  0% 0.00/450k [00:00<?, ?B/s]
100% 450k/450k [00:00<00:00, 67.7MB/s]
```
!unzip /content/adult-census-income.zip
```
Archive:  /content/adult-census-income.zip
  inflating: adult.csv               
```
df = pd.read_csv("/content/adult.csv")
```

## step 2 : Problem description
![anh1](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh1.jpg)

you see this data not null file

![anh2](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh2.jpg)
![anh3](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh3.jpg)
![anh4](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh4.jpg)
![anh5](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh5.jpg)
![anh6](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh6.jpg)
![anh7](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh7.jpg)
![anh8](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh8.jpg)
![anh9](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh9.jpg)
![anh10](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh10.jpg)
![anh11](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh11.jpg)

But this data have some missing values "?"

## step3 : the exploratory data analyst

![anh14](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh14.jpg)
 the data have age of values between from 20-60,the values of 60+ is very few

![anh15](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh15.jpg)


![anh16](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh16.jpg)

![anh17](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh17.jpg)

![anh18](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh18.jpg)

![anh19](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh19.jpg)


## step 4 : preprocessing data, build model
![anh12](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh12.jpg)
you see this data not null file but this have some value is "?" 
So we fill it by mod value because the value of data is imbalance.
```
df = df.replace('?', np.nan)
nan_cols = [i for i in df.columns if df[i].isnull().any()]
for col in nan_cols:
  df[col].fillna(df[col].mode()[0], inplace=True)
```
#### label all feature
```
from sklearn.preprocessing import LabelEncoder
for col in df.columns:
  if df[col].dtypes == 'object':
    encoder = LabelEncoder()
    df[col] = encoder.fit_transform(df[col])
```
#### chose some feature have affection of income
```
corr = df.corr()
mask = np.zeros_like(corr)
mask[np.triu_indices_from(mask)] = True
plt.style.use('default')
plt.figure(figsize=(17, 8))
ax = sns.heatmap(corr,cmap='RdYlGn', mask=mask, vmax=.3,annot=True)
plt.show()
```

![anh13](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh13.jpg)

![anh21](https://github.com/Tdpro1612/tutorial_data_science/blob/f384eb9f4916096c0123f19661acad7151a5a67e/dt/anh22.jpg)

you can see have 8 feature have affection of income : 'age','education','education.num','race','sex','capital.gain','capital.loss','hours.per.week' (>0.05)

#### build model
we build all model to see this
- linear regression

## step 4 : show accuracy,score,fine tuning model
