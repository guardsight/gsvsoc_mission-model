# How-To Develop An Incident Response Report Using GitHub-Sphinx-RTD
### Mission Model Using GitHub-Sphix-RTD

#### Prologue

GuardSight analysts use a Mission Model as a systematic approach for the objectives of containment, eradication, and recovery during incident response. One component of this approach includes developing content in an iterative manner to describe the adversary compromise as well as the allied response. The aggregated content ultimately results in an after action report. Producing the report during the response has a number of benefits including memorializing in near real-time, accuracy of observations and collections, and exactness of knowledge transfers when transitioning between analysts in order to combat response fatigue. This document discusses a mechanism for developing the Mission Model content using the revision control hosting system [Github](https://www.github.com), use of the [Sphinx](http://www.sphinx-doc.org/en/master/) documentation generator, and use of the software hosting system [Read the Docs](https://readthedocs.com/).


![img](images/gh.mm.1.png)

#### Prerequisites

1. Familiarity with contributing to Github
1. Authorized access to Github
1. Familiarity with publishing documentation using Sphinx
1. Local builds require Sphinx software
   ```bash
   $ pip install sphinx sphinx-autobuild
1.  Authorized access to Read the Docs for business **private** hosting

	* Non-redacted public postings of after action reports is probably **not smart** - readthedocs.com is **private** - readthedocs.io is **public**
