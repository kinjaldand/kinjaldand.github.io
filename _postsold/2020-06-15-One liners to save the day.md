---
layout: post
title: One Liners to save the day!
--- 

Scenario 1:

Lets say you edit a .sh file in windows and copy it to linux.
This introduces carriage returns in .sh files causing trouble in running them.

Solution: perl -pi -e 's/\r//' scriptfilename

Ref: https://stackoverflow.com/questions/15520339/how-to-remove-carriage-return-and-newline-from-a-variable-in-shell-script

####----------------------------------------------------------------------------------------####

Scenario 2:

Is-there-any-way-to-check-history-only-of-current-session

Solution:

history -a this_session.history
~$ cat ./this_session.history 

Ref: https://askubuntu.com/questions/1026997/is-there-any-way-to-check-history-only-of-current-session

Scenario 3:
Kubectl delete pod with name filter

kubectl get pods -o name | grep mnist | xargs kubectl delete

Scenario 4:
stop-pip-from-failing-on-single-package-when-installing-with-requirements-txt

Linux:
cat requirements.txt | sed -e '/^\s*#.*$/d' -e '/^\s*$/d' | xargs -n 1 pip install

Windows:
FOR /F %k in (requirements.txt) DO ( if NOT # == %k ( pip install %k ) )

Source:
https://stackoverflow.com/questions/22250483/stop-pip-from-failing-on-single-package-when-installing-with-requirements-txt





