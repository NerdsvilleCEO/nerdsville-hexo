---
title: Chef Private Key Vault
date: 2016-07-31 14:38:32
tags: chef, security, projects
---

## Problem
  I wanted the capability to allow a user of the chef server to upload their private key encrypted and only them to be able to decrypt it. 
  
  Additionally, I wanted to create a role for each individual user which would need to be added to the node in order to for the node to be allowed to decrypt the content.
  
## Solution
  Chef Vault turns out to be exactly what I needed to achieve this. It was however quite a pain to get it working how I wanted it to.
  
  The first part involves intervention by the admin, this involves creating a user for the team member, and creating a role that only they have permission to modify.
  
  In order to get the private key in a JSON format for chef vault to use, I had to do the following:
  
  ```
  #!/bin/sh
  key=$(tr '\n$' ':' < $1 | gsed 's/:/\\n/g')
  printf '{\n "github_key": "%s"\n}\n' "$key"
  ```
  
  This replaces all newlines with the literal "\n" and prints it out into a JSON text which can then be outputted to a file.
  
  Next, the user would need to clone down the chef repository and create the new vault:
  
  ```
    cd chef-repo
    knife vault create keys USER -S 'USER_ROLE' -A 'USER' -J json_file.json
    knife upload data_bags 
  ```
  Finally, the node can be assigned the role to it's runlist et voila, it can now pull down the encrypted content for the user :)
  
  But... this isn't without issues
  
## Issues
The problem with this solution is that Chef does not handle ACL's very well for situations such as this.

In fact, a node that is Chef managed is able to modify it's own runlist! 

For this reason, at first I thought that I hit a roadblock with Chef, and started thinking I better move to another platform if Chef can not handle access privillege separation properly.

See this [Github Chef Issue](https://github.com/chef/chef/issues/5153) for validation on this issue with ACL.

I am currently working on a solution which will resolve this issue, so stay tuned :D