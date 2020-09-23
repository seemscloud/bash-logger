# BASH Logger

```bash
#!/bin/bash

function formatter {
  DATE="`date +\"%Y-%m-%d %H:%M:%S,%3N\"`"

  echo -e "$DATE $1 $2"
}

# for i in `seq 120` ; do echo -e "\e[${i}mMESSAGE\e[m $i" ; done
function messager {
  if [ "$2" == "error" ] ; then
    COLOR_NUMER="91"
    formatter "\e[${COLOR_NUMER}mERROR\e[m\t" "${1}"
  elif [ "$2" == "success" ] ; then
    COLOR_NUMER="92"
    formatter "\e[${COLOR_NUMER}mSUCCESS\e[m\t" "${1}"
  elif [ "$2" == "warning" ] ; then
    COLOR_NUMER="93"
    formatter "\e[${COLOR_NUMER}mWARNING\e[m\t" "${1}"
  elif [ "$2" == "info" ] ; then
    COLOR_NUMER="96"
    formatter "\e[${COLOR_NUMER}mINFO\e[m\t" "${1}"
  else
    formatter "MESSAGE\t" "${1}"
  fi
}

function logger {
  [ "$#" -ge 1 ] && [ "$#" -le 2 ] || exit

  [ "$#" -eq "1" ] && messager "$1"
  [ "$#" -eq "2" ] && messager "$2" "$1"
}
```
