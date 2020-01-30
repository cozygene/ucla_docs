---
description: >-
  Woohooo congratulations for starting at UCLA research! This is a wiki to help
  you get everything you need
---

# OnBoarding @ UCLA

## Applying for a Hoffman2 Cluster account

Hoffman2 Cluster account creation/management is processed though an in-house web application "Grid Identity Manager". GIM needs you to have a UCLA Logon ID for authentication.

For a UCLA Logon ID: [see Getting a UCLA Logon ID](getting-a-ucla-logon-id..md). 

After you have a UCLA Logon go to [https://www.hoffman2.idre.ucla.edu/getting-started](https://www.hoffman2.idre.ucla.edu/getting-started). 

Click `"New User Registration"`. The UCLA Federated Authentication Service will ask you for your UCLA Logon ID and password. 

Fill in the information from the form. 

{% hint style="info" %}
For choose a faculty sponsor. Choose the PI that you are working for from the pull-down list of sponser. I.e. For Professor Eran Halperin - you would select Eran Halperin from the drop down list. 
{% endhint %}

Now that you have a Hoffman2 Cluster, let us get everything installed. 

### Hoffman2 Basics/Terminal Setup.

* **tmux:** tmux allows multiple terminal sessions to be accessed simultaneously in a single window. ****

  * tmux is already loaded in hoffman. So in your bashrc append the line 

  ```text
  module load tmux
  ```

* **CUDA**: CUDA is a parallel computing platform and application programming interface model created by Nvidia.

```text
module load cuda/10.0 # for cuda 10.0
```

* To see all available programs already installed on hoffman2, type in your bash: `module av`

{% hint style="info" %}
Note that your home directory has approximately 20GB worth of storage. Thus it is recommended to work in your groups directory or in the scratch directory \(more to follow\). 
{% endhint %}

* **conda:** conda is a package that allows you to install most python packages. 

  * You can use `module load conda` however this will install it in your home directory. To install it somewhere else, you can run this script: [Anaconda3-2019.10-Linux-x86\_64.sh](https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh). 
  * To get it on hoffman from the command line run: 
  * ```text
    wget https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh; 
    bash Anaconda3-2019.10-Linux-x86_64.sh
    ```
  * For the path, install it in the project directory \(i.e. `/u/project/{project_you_belong_to}/{your_username}/anaconda3`



At the end your `bashrc` should look similar to this 

{% code title=".bashrc" %}
```bash
bash Anaconda3-2019.10-Linux-x86_64.sh # Source global definitions
if [ -f /etc/bashrc ]; then
    . /etc/bashrc
fi

# added 20110721- BMP
# User specific aliases and functions
umask 0022
PS1="(`uptime`)\n\u@\h:\w$ "

module load tmux
module load ffmpeg/1.1.1
module load curl/7.49.1
module load cuda/10.0

# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/u/project/halperin/gnahum12/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/u/project/halperin/gnahum12/anaconda3/etc/profile.d/conda.sh" ]; then
        . "/u/project/halperin/gnahum12/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="/u/project/halperin/gnahum12/anaconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
```
{% endcode %}

### Useful Terminal Shortcuts: 

{% code title=".bash\_profile" %}
```bash
# some more ls aliases
alias ll='ls -alF'
alias la='ls -A' # list all
alias l='ls -CF' # list
alias clr='clear' # clear terminal
alias glog='git log --graph --simplify-by-decoration --all' # get the graph from git.
alias py="python" # run python
alias scratch="cd $SCRATCH"
```
{% endcode %}

### Useful Git Shortcuts and using Git: 

In your `.gitconfig` file. 

{% code title=".gitconfig" %}
```text
[alias]
	up = checkout
	st = status
  unstage = reset HEAD --
  amend = commit --amend
[user]
  name = <your name> 
  email = <your@email.com> 
  
```
{% endcode %}

Git is a version control software. Some basics are: 

{% code title="All local to the system. " %}
```text
git add filename.extension
git status # or `git st` if you have the above config file. 
git commit -m "message here of work you did" 
```
{% endcode %}

Hoffman can only connect using ssh \(i.e. not http dumps\)   
Therefore in order to connect you need to have an ssh-keygen ready. 

To do this you need to create/go to the `.ssh` directory 

```text
# if .ssh is already created: 
cd ~/.ssh; 
```

```text
# if .ssh doesn't exist: 
mkdir ~/.ssh; cd ~/.ssh; 
```

Then run 

```text
ssh-keygen # this will create a public and private key. 
```

Follow this guide to link it to [Github](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)/[Bitbucket](https://confluence.atlassian.com/bitbucket/set-up-an-ssh-key-728138079.html)/[Other](https://www.google.com/search?q=how+do+I+connect+%7BINSERT+PLATFORM%7D+with+ssh+keys&rlz=1C5CHFA_enUS704US704&oq=how+do+I+connect+%7BINSERT+PLATFORM%7D+with+ssh+keys&aqs=chrome..69i64.19604j0j7&sourceid=chrome&ie=UTF-8).

 This will now allow you to push to a remote branch: 

```text
git remote -v # this will give you the name + url. Get the name of the ssh
git push {name} {branch} 
```

For more git advice/practice please see: [Learn Git Branching](https://learngitbranching.js.org/) or [Visualizing Git](http://git-school.github.io/visualizing-git/)

### Transferring Content to Hoffman2 Cluster

#### Using scp 

* `scp`, standing for secure copy paste, allows you to transfer files between an ssh server and your local computer. It’s a useful command if you set up ssh keys so that you don’t have to enter your password every time you want to transfer files.
* To send a file from local to cluster :

`scp filename.extension username@hoffman2.idre.ucla.edu:∼/directorytosavein/`

* To send multiple files from local to cluster

`scp filename1.extension filename2.extension username@hoffman2.idre.ucla.edu:∼/directorytosavein/`

* To send directory with all files from local to cluster: 

`scp -r /local/path/to/directory2send username@hoffman2.idre.ucla.edu:~/path/to/where/you/want`

* To send a file from cluster to local

`scp username@hoffman2.idre.ucla.edu:∼/directoryitsin/filename.extension ./path/to/where/you/want/`

* To send multiple files from cluster to local

`scp username@hoffman2.idre.ucla.edu:∼/directoryitsin/{filename1.extension,filename2.extension} ./path/to/where/you/want/`

* To send directory with all files from cluster to local

`scp -r username@hoffman2.idre.ucla.edu:∼/directorytosend ./path/to/where/you/want`



### Getting MedNet account: 

Follow these steps: 

* Complete the following forms: 



Instructions:

{% file src=".gitbook/assets/form-access-request-non-employee.pdf" caption="Access Request Form " %}

1. Access Request Form \(above\) 
   1. Fill in fields \#1 - \#8
   2. For field \#2, put in “Research Collaborator”
   3. Check the box for “New Application”
   4. For field \#6, put in “Computational Medicine” for Sponsoring Department and “Eran Halperin” for Manager/Supervisor
   5. For field \#7, put in one year from application date
   6. For field \#8, check the box for “AD Domain” and “VPN”
   7. Sign at the “Applicant Signature” section and date. Please make sure not to sign in field \#9 but the section above \(you’re not the authorizer\). Hand signatures only, no e-signatures accepted.
2. HIPAA completion certification 
   1. [https://www.uclahealth.org/compliance/hipaa-education-training](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.uclahealth.org%2Fcompliance%2Fhipaa-education-training&data=02%7C01%7CEileenLu%40mednet.ucla.edu%7Cac47032161b340a66e7b08d4d4537866%7C39c3716b64714fd5ac04a7dbaa32782b%7C0%7C0%7C636366903290168174&sdata=dp8J1xHl4cC25zn%2B3RV3FEvo2SukLVnqcr%2B1mcL4CnQ%3D&reserved=0)
   2. Click on the first link on the page: Health Insurance Portability and Accountability Act \(HIPAA\) Privacy & Security Workforce Training
   3. Upon completion, please make sure to save/print the resulting certificate of completion as a PDF

{% file src=".gitbook/assets/confidentiality-nonworkforce-members.pdf" caption="Confidentiality Agreement for non-workforce" %}



1. Confidentiality Agreement for non-workforce \(above\) 
   1. Fill in all fields
      1. Sign and date. Hand signed only, no e-signatures accepted.
2. Return the three completed pieces of paperwork to the authorizer listed above and let them know you need this person to have a Mednet account. Feel free to cc'  [Clifford \(CKravit@mednet.ucla.edu\)](mailto:CKravit@mednet.ucla.edu)

### Accessing AWS: 

{% hint style="info" %}
Note that you only have access to AWS if you have a Mednet account as well. 

In order to get a Mednet account please see above. 
{% endhint %}

Email: [Clifford \(CKravit@mednet.ucla.edu\)](mailto:CKravit@mednet.ucla.edu) or [Mark Lucas\(MLucas@mednet.ucla.edu\)](mailto:MLucas@mednet.ucla.edu) requesting access to AWS GPU/CPU/Control. 

Note that once you have access, please reserve a port here: [https://docs.google.com/spreadsheets/d/1ipRkinTUX\_hkW2AaV9FIQiIvoGFNF2qbVbKZh3LuJBE/edit?usp=sharing](https://docs.google.com/spreadsheets/d/1ipRkinTUX_hkW2AaV9FIQiIvoGFNF2qbVbKZh3LuJBE/edit?usp=sharing)

For easy access, place this in your `.bashrc` wherever you will be sshing into aws. 



{% code title=".bashrc " %}
```bash
alias ehr='ssh -Y CHANGE@HS-PSTOPRSKA-RP.AD.MEDCTR.UCLA.EDU -J CHANGE@internal-bastionelb-research-prod-2054024064.us-west-2.elb.amazonaws.com -L CHANGE:127.0.0.1:CHANGE'
alias awsb='ssh -Y CHANGE@HS-PSTOPRSKA-RP.AD.MEDCTR.UCLA.EDU -J CHANGE@internal-bastionelb-research-prod-2054024064.us-west-2.elb.amazonaws.com -L CHANGE:127.0.0.1:CHANGE'
alias awscont='ssh -Y CHANGE@HS-PSTOPRSKC-RP.AD.MEDCTR.UCLA.EDU -J CHANGE@internal-bastionelb-research-prod-2054024064.us-west-2.elb.amazonaws.com'
alias awsgpu='ssh -Y CHANGE@HS-PSTOPRSKG-RP.AD.MEDCTR.UCLA.EDU -J CHANGE@internal-bastionelb-research-prod-2054024064.us-west-2.elb.amazonaws.com -L CHANGE:127.0.0.1:CHANGE'
```
{% endcode %}

In the `awscont` bashrc place: 

{% code title=".bashrc in awscont" %}
```bash
alias status='aws ec2 describe-instance-status --region us-west-2 --include-all-instances --instance-id i-04d0da8958d7425db --output text'
alias gpu_start='aws ec2 start-instances --region us-west-2 --instance-ids i-04d0da8958d7425db --output text'
alias gpu_stop='aws ec2 stop-instances --region us-west-2 --instance-ids i-04d0da8958d7425db --output text'
```
{% endcode %}



