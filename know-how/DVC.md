# Data Version Control (DVC)
<p align="center">
  <img src="https://miro.medium.com/max/700/1*gN7Xru3A-PTavPI6adpJPQ.png?raw=true" alt="DVC BAnner"/>
</p>

## Initializing DVC and configuring remote storage

Inside a git versioned ${PROJECT}. 

```
# Python 3.8+ is needed to get the latest version of DVC
pip install dvc
# Inicializes DVC and Git 
dvc init
# We can see that a .dvc folder was created. This will be used by DVC to store & track the different data versions
git status
git commit -m "dvc logs files"
```
Next, we utilize the add the data storage For more options see [DVC Remote](https://dvc.org/doc/command-reference/remote)

```
dvc remote add -d ${DVC_REMOTE} ${DVC_REMOTE_URL}

# Set up remote configurations
dvc remote modify ${DVC_REMOTE} 
dvc remote modify ${DVC_REMOTE} custom_auth_header X-Token 

dvc remote modify --local  ${DVC_REMOTE} password ${GIT_ACCESS_TOKEN}
cat .dvc/config
git add .
git commit -m "sets remote storage configuration"
```

## Data versioning with DVC 


```
FILENAME=large_file.txt
echo " Example Large file" > ${FILENAME} 
#  Should  create a ${FILENAME}.dvc files and updates the .gitignore 
dvc add ${FILENAME}.txt 
git add .
git commit -m "adds example file ${FILENAME}"
```
We can later modify the file data and  that git does not track the file 

```
echo "Add more example data" >> ${FILENAME} 
dvc add ${FILENAME} 
git commit -a -m - "modify example file ${FILENAME}" 
```

### Restore files DVC and Git older versions
To restore a file  to its former glory, use git checkout from the last known commit, which is HEAD. This command can be used anytime to go back again to the latest version using:

```
git checkout HEAD ${FILENAME} 
dvc checkout 
git commit -m 'restoring ${FILENAME} from last commit.'
```

To recover a file from a specific (older) git commit. Select the desired commit sha from logs 

```
git log –oneline
sha=<commithash>
```
Go back to that Git commit DVC file history. DVC recognize the updated  ${FILENAME}.dvc and brings the old data version

```
git checkout ${sha} ${FILENAME}.dvc
dvc checkout 
git commit -m 'restoring filename from ${sha} commit.'
```

### Updating and versioning data across projects

```
git remote -v
git remote add dataset ${GIT_CLONE_URL}
#Update all branches from all remotes 
git fetch --all
git checkout dataset/${GIT_BRANCH} ${FILENAME}.dvc 
git status
git add .
git commit -m "updates new ${FILENAME}.dvc file".
# New data version is downloaded from  a DVC remote
dvc pull 
```
  
##  DVC workflow

Use `dvc dag` to plot the pipeline graph


### Creating a dvc pipeline

In the `params.yaml` we include those parameters that will be used for experimenting in our ML pipeline

For each step of the pipeline we can use the dvc run command. This command results in a `dvc.yaml` and a `dvc.lock` file.

e.g.
```
$ dvc run
-n train_model \ # name
-p train.model, train.epochs \ # params
-d data src/train.py \ # dependencies
–o model.pt \ # output
-m metrics.json \ # output metrics
--plots track/history.csv \ 
python3 src/train.py # cmd

```

### DVC Experiments:


```
# dvc experiments allows to queue a number of experiments and run them sequentially
# i.e.

dvc exp run --queue -S train.max_depth=9
dvc exp run --queue -S train.max_depth=10 -S train.learning_rate=0.001

dvc queue start 

dvc exp show # Shows a table with the current experiments
dvc exp apply <exp-hash> # Applies changes to our workspace
dvc exp branch <exp-hash> <branch_name> # To promote an experiment to a new Git branch 

```

### Other Useful commands



```
# dvc will store all data versions in cache. Sometimes it is a good idea to clear up the cache.
# As shown below we can use the garbage collector (gc) command to keep only the data used in the current workspace.

du -sh .dvc/cache # We can check the memory that cache is taking
dvc gc --workspace # Clear-up cache
du -sh .dvc/cache # Check how much memory we released

```

