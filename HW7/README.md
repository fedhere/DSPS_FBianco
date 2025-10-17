# READING 

10/22/2025 midinght

Helping Students Use GenAI to Think, Learn, and Grow--Without Losing the Skills That Matter Most. - may need to be logged in into UD to access

https://research.ebsco.com/c/5yi2sn/viewer/pdf/ylzl76v2g5?auth-callid=10c221d7-93f4-44c1-a3fa-756f4a087129


# READING 
Read the first five pages of https://arxiv.org/pdf/1910.10045.pdf through section 2.1 included

# Assignment: 

## 1. Kaggle data scaling and clustering

- Task 1: make a kaggle account and set up your API (see below)
- Task 2: read in the data for the World Happniess Dataset https://www.kaggle.com/datasets/unsdsn/world-happiness/data - use the 2025 data
### scaling
- Task 3: For each numerical column X, prepare a column that is a _minmax_ version of X and a version that is the _standardized_ version of X, store them in the same or another dataframe (your choice) as, for example  X_minmax and  X_standardized (choose the variable or dataframe names you want, but make them meaningful and descriptive!)
- Task 4: For each numerical column pair X and Y make a scatter plot of Y vs X with the data as is read in, a scatter plot of Y_minmax vs X_minmax, and a scatter plot of Y_standardized and X_standardized
### clustering
- Task 5: Using KMeans clustering, cluster the scaled numerical features (choose either scaling) that are used to calculate the score: 'Family', 'Health (Life Expectancy)',
       'Freedom', 'Trust (Government Corruption)', 'Generosity',
       'Dystopia Residual' into 3 clusters
- Task 6: Make a scatter plot with the cluster (0, 1, or 2) on the X axis, and the Happiness score _with its errorbar_ on the Y axis and, as usual, comment on the figure your _what, how, wow_
- Task 7: extra credit for 461, required for 661: repeat for 2, 3, 4, 5 , 6, 7 and make a plot of KMeans intracluster variance vs the number of clusters (respectively Y and X) and discuss if this plots allows for a robust selection of the correct number of clusters
  

### Here is how to make a kaggle account and set up your API
Register for an account at kaggle.com, all the work we will do together in class as well as your next homework will require it. To register do the following:

go to kaggle.com

click on “Register” in the upper right corner select either Register with Google, or Register with your email (it’s up to you)
follow the instructions provided by kaggle to create an account (enter your email address, create a password, and choose a username), all of which are up to you, this will be your account after all
Make sure that at the end, you have an account that you can log in with, and be logged in and ready next class


- Go to https://www.kaggle.com/ and sign in

- Click on the icon of your avatar on the top right
  <img width="929" alt="Screen Shot 2023-10-21 at 1 23 01 PM" src="https://github.com/fedhere/FDSFE_FBianco/assets/1696902/2d1a6c89-2230-4398-a9fe-c16fa4dd053e">

- select "Settings" from the drop menu
  <img width="627" alt="Screen Shot 2023-10-21 at 1 28 43 PM" src="https://github.com/fedhere/FDSFE_FBianco/assets/1696902/2d92102f-79ab-4eb7-ae86-f71ca14d706a">

- scroll down to API and click create New API Token. This will download a json file on your computer
- 
<img width="654" alt="Screen Shot 2023-10-21 at 1 24 10 PM" src="https://github.com/fedhere/FDSFE_FBianco/assets/1696902/8ab3ae90-9c4d-4ee9-8442-b28dce975fd4">
<img width="855" alt="Screen Shot 2023-10-21 at 1 23 57 PM" src="https://github.com/fedhere/FDSFE_FBianco/assets/1696902/0de116d5-56c2-4024-877d-518972de8a16">

- open google drive at https://drive.google.com/drive/u/0/my-drive in your browser
- upload (e.g. dragna and drop) the kaggle.json file from your laptop to the drive

  <img width="1469" alt="Screen Shot 2023-10-21 at 1 26 51 PM" src="https://github.com/fedhere/FDSFE_FBianco/assets/1696902/c7e27015-34ad-41a8-9f0f-5a816ff5c504">

- the rest of the information are to be done on your colab notebook: follow accordingly

```
# this mounts your google drive
from google.colab import drive
drive.mount("/content/gdrive")

# this gets you to your drive folder
cd gdrive/My\ Drive/

# this makes sure the file is there: this cell should return "kaggle.json"
ls kaggle.json

# this limits who can view and make changes who can access this file.
!chmod 600 kaggle.json

# this reads in the file and stores it into the system variables of your colab sessions which allows you to connect programmatically to the kaggle platform
envs = json.load(open("kaggle.json", "r"))
os.environ["KAGGLE_USERNAME"] = envs['username']
os.environ["KAGGLE_KEY"] = "e60b57c215e877e01a22375a3058eec1"#envs['key']
```

once your API is set up and loaded in your colab notebook you can use it to read in data from kaggle https://codesolid.com/kaggle-datasets/

