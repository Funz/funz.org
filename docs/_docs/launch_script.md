---
title: Advanced scripting to launch simulations
permalink: /docs/launch_script/
---

This section gives useful tips to build a script that run your code in batch mode on a no-display environment, whenever the code requires a graphical display to produce views of the model, for instance.

It is based on the **Xvfb** and **xdotool** Linux comands (assumed to be installed previously).

```bash
#!/bin/bash

# to customize with your own installation path
CODE_HOME=...

...

# convert EOL from windows to unix
INPUT_FILE=`basename $1`
mv -f $INPUT_FILE $INPUT_FILE.orig
tr -d '\r' < $INPUT_FILE.orig > $INPUT_FILE

# to check if DISP display command is asked. In this case, uses Xvfb and xdotool to get .ps/... and continue calculation.
# need packages Xvfb, xdotool, x11-xkb-utils. Xvfb and xdotool to be in PATH
DISP=`grep DISP * | wc -l`

PROG=$CODE_HOME/bin/$CODE

if [ ! "$DISP" == "0" ]; then
  for i in `seq 1 10`; do if [ ! -e /tmp/.X$i-lock ]; then NDISPLAY=$i; break; fi done
  Xvfb :$NDISPLAY &
  PID_XVFB=$!
  echo $PID_XVFB >> PID
  echo "export DISPLAY=:"$NDISPLAY".0"
  export DISPLAY=:$NDISPLAY".0"
fi

$PROG -d $INPUT_FILE > $INPUT_FILE.out &
PID_CODE=$!
# allows Funz to kill CODE when needed
echo $PID_CODE >> PID

if [ ! "$DISP" == "0" ]; then
  PRINTED_DISP=`grep "checking association" $INPUT_FILE.out`
  PRINTED_ERROR=`grep "error" $INPUT_FILE.out``grep "entry not found" $INPUT_FILE.out`
  while [ "$PRINTED_DISP" == "" ] && [ "$PRINTED_ERROR" == "" ]
  do
    sleep 3
    echo "Return"
    xdotool key Return # virtually press "Return" key to continue on DISP loop
    PRINTED_DISP=`grep "checking association" $INPUT_FILE.out`
    PRINTED_ERROR=`grep "error" $INPUT_FILE.out``grep "entry not found" $INPUT_FILE.out`
  done

  kill -2 $PID_XVFB
  rm -f /tmp/.X$NDISPLAY-lock
fi

wait $PID_CODE

# restore initial input file
mv -f $INPUT_FILE.orig $INPUT_FILE

rm -f PID

PRINTED_ERROR=`grep "error" $INPUT_FILE.out``grep "entry not found" $INPUT_FILE.out`
if [ ! "$PRINTED_ERROR" == "" ]; then
		echo "Calculation failed. exiting."
		#echo " simulation time -1" >> $INPUT_FILE.out # turn around no longer needed in 1.1
    	exit 0
fi
```