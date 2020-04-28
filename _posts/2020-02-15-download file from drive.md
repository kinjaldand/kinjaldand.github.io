---
layout: post
title: Download large files on google drive using curl
---

Follow below linux commands and Voila!

ggID='put_googleID_here'  
ggURL='https://drive.google.com/uc?export=download'  
filename="$(curl -sc /tmp/gcokie "${ggURL}&id=${ggID}" | grep -o '="uc-name.*</span>' | sed 's/.*">//;s/<.a> .*//')"  
getcode="$(awk '/_warning_/ {print $NF}' /tmp/gcokie)"  
curl -Lb /tmp/gcokie "${ggURL}&confirm=${getcode}&id=${ggID}" -o "${filename}" 

REF: <a href='https://stackoverflow.com/questions/25010369/wget-curl-large-file-from-google-drive'>Stack Overflow</a>