# How-To Develop An Incident Response Report Using GitHub-Sphinx-RTD
### Mission Model Using GitHub-Sphix-RTD

#### Prologue

GuardSight analysts use a Mission Model as a systematic approach to achieve the objectives of containment, eradication, and recovery during incident response. One component of this approach includes developing content in an iterative manner to describe the adversary compromise as well as the allied response. The aggregated content ultimately results in an after action report. Producing the report during the response has a number of benefits including memorializing in near real-time, accuracy of observations and collections, and exactness of knowledge transfers when transitioning between analysts in order to manage response fatigue. This document discusses a mechanism for developing the Mission Model content using the revision control hosting system [Github](https://www.github.com), use of the [Sphinx](http://www.sphinx-doc.org/en/master/) documentation generator, and use of the software hosting system [Read the Docs](https://readthedocs.com/).

![img](images/gh.mm.2.png)

![img](images/gh.mm.3.png)


#### Prerequisites

1. Familiarity with contributing to Github
1. Authorized access to Github
1. Familiarity with publishing documentation using Sphinx
1. Sphinx software for Local builds (optional but recommended)
   ```bash
   pip install sphinx sphinx-autobuild
1.  Authorized access to Read the Docs for business **private** hosting

	* Non-redacted public postings of after action reports is probably **not smart** - readthedocs.com is **private** - readthedocs.io is **public**
1. Github Settings
   ```bash
   vi ~/.gitconfig
   [user]
	   name = myName
	   email = myName@myEmailDomain

#### Instruction

![img](images/gh.mm.1.png)

##### Bootstrap

1. Create a new repo that will  contain the after action report (**notice the private key has its boolean value set to true**)
   ```bash
   cd ~/sandbox/code/github
   MISSION=$(date +'MISSION-%Y%m%d-1')
   MYORG=myOrganization
   curl -u $(grep name ~/.gitconfig | awk '{print $NF}') -d '{ "name": "'${MISSION}'", "description": "Incident Response After Action Report", "private": true, "has_wiki": false }' https://api.github.com/orgs/${MYORG}/repos
   Enter host password for user 'myName':
1. Duplicate a template repo without forking it and mirror-push its contents into the new repo
   ```bash
   git clone --bare git@github.com:guardsight/gsvsoc_mission-model MISSION-BOOTSTRAP
   cd MISSION-BOOTSTRAP/
   git push --mirror git@github.com:${MYORG}/${MISSION}
   cd .. && rm -rf MISSION-BOOTSTRAP
1. Create a development branch and incorporate the remote repo into the local branch
   ```bash
   git clone git@github.com:${MYORG}/${MISSION} ${MISSION}
   cd ${MISSION}
   git checkout develop
   git pull origin develop
   cd docs
1. Replace some default content
   ```bash
   sed -i "s/MISSION-YYYYMMDD-n/${MISSION}/g" source/index.rst source/meta.txt
   ... Replace the GuardSight copyright to ${MYORG}
   ... Replace docs/source/meta-logo.png with ${MYORG} logo
   
IT **IS PERMISSABLE** TO REPLACE THE LOGO AND COPYRIGHT NOTICE IN THE CLONED ${MISSION} AND THE GUARDSIGHT PERMISSION NOTICE **IS NOT REQUIRED** TO BE INCLUDED IN THE CLONED ${MISSION} OR ANY PORTION OF THE AFTER ACTION REPORT
   
##### Edit <=> Commit

1. Develop -> Commit -> Push
   ```bash
   cd ${MISSION}
   git checkout develop; git pull origin develop
   emacs -nw source/index.rst
   ...meta.txt...
   ...summary.rst...
   git commit -a -m "Mission update"
   git push --tags origin develop
1. Merge Into Master -> Push
   ```bash
   git checkout master
   git merge develop
   git push --tags origin master
   git checkout develop
   
   
##### Local Build

1. Make up the build
   ```bash
   cd ${MISSION}/docs
   make html
   google-chrome build/html/index.html
	
	
##### Read the Docs For Business Build

1. Import the repo into RTD
	```bash
	google-chrome https://readthedocs.com/dashboard/import/?
	
	... Import the project
	... Email received from Github: [GitHub] A new public key was added to ${MYORG}/${MISSION}
	
	google-chrome https://${MYORG}-$(echo ${MISSION} | tr [[:upper:]] [[:lower:]]).readthedocs-hosted.com/en/latest/
	

