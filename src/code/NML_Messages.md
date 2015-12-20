NML Messages
============

EMC OPERATOR
------------

### EMC\_OPERATOR\_ERROR\_TYPE

Description, NML Type: textual error message to the operator, 11

Written From: emccanon.cc, iosh.cc

Read To: emctaskmain.cc, emcsh.cc

Parameter, Type: \[error, char\[LINELEN\]\]

### EMC\_OPERATOR\_TEXT\_TYPE

Description, NML Type: textual information message to the operator, 12

Written From: emctaskmain.cc

Read To: emctaskmain.cc, emcsh.cc

Parameter, Type: \[text, char\[LINELEN\]\]

### EMC\_OPERATOR\_DISPLAY\_TYPE

Description, NML Type: URL or filename of a document to display, 13

Obs: not used, only read

Written From: none

Read To: emctaskmain.cc, emcsh.cc

Parameter, Type: \[display, char\[LINELEN\]\]

EMC NULL, SET, DEBUG, & SYSTEM
------------------------------

### EMC\_NULL\_TYPE

Description, NML Type: used to reset serial number to original, 21

Written From: thisQuit (emcsh.cc)

Read To: emctaskmain.cc

Parameter, Type: none

### EMC\_SET\_DEBUG\_TYPE

Description, NML Type: sets debug level, 22

Written From: emcIoSetDebug (iotaskintf.cc), sendDebug (emcsh.cc)

Read To: emctaskmain.cc, ioControl.cc

Parameter, Type: \[debug, int\]

### EMC\_SYSTEM\_CMD\_TYPE

Description, NML Type: execute a system command, 30

Written From: user\_defined\_add\_m\_code (emctask.cc)

Read To: emcSystemCmd (emctaskmain.cc)

Parameter, Type: \[string, char\[LINELEN\]\]

EMC AXIS
--------

### EMC\_AXIS\_SET\_AXIS\_TYPE

Description, NML Type: axis type to linear or angular, 101

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[axisType, unsigned char\]

### EMC\_AXIS\_SET\_UNITS\_TYPE

Description, NML Type: units conversion factor, 102

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[units, double\]

### EMC\_AXIS\_SET\_GAINS\_TYPE

Description, NML Type: Set the PID gains, 103

Obs: currently not used in EMC2, needs to go to HAL

Written From: none

Read To: emctaskmain.cc

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[p, double\] \[i,double\] \[d, double\] \[ff0, double\] \[ff1, double\] \[ff2, double\] \[backlash, double\] \[bias, double\] \[maxError, double\]

### EMC\_AXIS\_SET\_CYCLE\_TIME\_TYPE

Description, NML Type: cycle time for the servo task, 104

Written From: none

Read To: emctaskmain.cc

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[cycleTime, double\]

### EMC\_AXIS\_SET\_INPUT\_SCALE\_TYPE

Description, NML Type: scale factor and offset for the position input, 105

Obs: currently if 0’ed, used only directly from iniaxis

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc) calls emcAxisSetInputScale (minimill|bridgeporttaskintf.cc) which sends EMCMOT\_SET\_INPUT\_SCALE
Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[scale, double\] \[offset, double\]

### EMC\_AXIS\_SET\_OUTPUT\_SCALE\_TYPE

Description, NML Type: scale factor and offset for the position output, 106

Obs: currently if 0’ed, used only directly from iniaxis

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc)

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[scale, double\] \[offset, double\]

### EMC\_AXIS\_SET\_MIN\_POSITION\_LIMIT\_TYPE

Description, NML Type: sets min limit, 107

Obs: also handled by iniaxis which directly calls emcAxisSetMinPositionLimit

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc) calls emcAxisSetMinPositionLimit (taskintf.cc) which sends EMCMOT\_SET\_POSITION\_LIMITS
Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[limit, double\]

### EMC\_AXIS\_SET\_MAX\_POSITION\_LIMIT\_TYPE

Description, NML Type: sets max limit, 108

Obs: also handled by iniaxis which directly calls emcAxisSetMaxPositionLimit

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc) calls emcAxisSetMaxPositionLimit (taskintf.cc) which sends EMCMOT\_SET\_POSITION\_LIMITS
Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[limit, double\]

### EMC\_AXIS\_SET\_MIN\_OUTPUT\_LIMIT\_TYPE

Description, NML Type: -, 109

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[limit, double\]

### EMC\_AXIS\_SET\_MAX\_OUTPUT\_LIMIT\_TYPE

Description, NML Type: -, 110

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[limit, double\]

### EMC\_AXIS\_SET\_FERROR\_TYPE

Description, NML Type: sets max following error, 111

Obs: also handled by iniaxis which directly calls emcAxisSetFerror

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc) calls emcAxisSetFerror (taskintf.cc) which sends EMCMOT\_SET\_MAX\_FERROR
Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[ferror, double\]

### EMC\_AXIS\_SET\_HOMING\_VEL\_TYPE

Description, NML Type: -, 112

Obs: in EMC2 those are SET\_HOMING\_PARAMS double home, double offset, double search\_vel, double latch\_vel, int use\_index, int ignore\_limits,

Written From: none

Read To: none

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[ferror, double\]

### EMC\_AXIS\_SET\_HOME\_TYPE

Description, NML Type: -, 113

Written From: none

Read To: none

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[homingVel, double\]

### EMC\_AXIS\_SET\_HOME\_OFFSET\_TYPE

Description, NML Type: -, 114

Written From: none

Read To: none

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[home, double\]

### EMC\_AXIS\_SET\_MIN\_FERROR\_TYPE

Description, NML Type: sets min following error, 115

Obs: also handled by iniaxis which directly calls emcAxisSetMinFerror

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcAxisSetMinFerror (taskintf.cc)
 which sends EMCMOT\_SET\_MIN\_FERROR

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[offset, double\]

### EMC\_AXIS\_SET\_MAX\_VELOCITY\_TYPE

Description, NML Type: sets max. velocity, 116

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[vel, double\]

### EMC\_AXIS\_INIT\_TYPE

Description, NML Type: -, 118

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\]

### EMC\_AXIS\_HALT\_TYPE

Description, NML Type: -, 119

Obs: not used, only read

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcAxisHalt (taskintf.cc)

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\]

### EMC\_AXIS\_ABORT\_TYPE

Description, NML Type: aborts motion on an axis (e.g. GUI jogs), 120

Obs: used from the GUI when stopping a manual jog

Written From: sendJogStop (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcAxisAbort (taskintf.cc)
 which sends EMCMOT\_AXIS\_ABORT

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\]

### EMC\_AXIS\_ENABLE\_TYPE

Description, NML Type: enables axis, 121

Obs: not used from tkemc & mini

Written From: sendAxisEnable (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcAxisEnable (taskintf.cc)
 which sends EMCMOT\_ENABLE\_AMPLIFIER

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\]

### EMC\_AXIS\_DISABLE\_TYPE

Description, NML Type: disable axis, 122

Obs: not used from tkemc & mini

Written From: sendAxisDisable (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcAxisDisable (taskintf.cc)
 which sends EMCMOT\_DISABLE\_AMPLIFIER

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\]

### EMC\_AXIS\_HOME\_TYPE

Description, NML Type: home an axis at current position, 123

Obs: used from tkemc & mini through emc\_home

Written From: sendHome (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcAxisHome (taskintf.cc)
 which sends EMCMOT\_HOME

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\]

### EMC\_AXIS\_JOG\_TYPE

Description, NML Type: jogs an axis continuously, 124

Obs: used on jogging

Written From: sendJogCont (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcAxisJog (taskintf.cc)
 which sends EMCMOT\_JOG\_CONT

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[vel, double\]

### EMC\_AXIS\_INCR\_JOG\_TYPE

Description, NML Type: jogs an axis with an increment, 125

Obs: used on jogging

Written From: sendJogIncr (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcAxisIncrJog (taskintf.cc)
 which sends EMCMOT\_JOG\_INCR

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\]
 incr, double\] \[vel, double\]

### EMC\_AXIS\_ABS\_JOG\_TYPE

Description, NML Type: jogs an axis with an absolute value, 126

Obs: not used, only read

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcAxisAbsJog (taskintf.cc)
 which sends EMCMOT\_JOG\_ABS

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[pos, double\] \[vel, double\]

### EMC\_AXIS\_ACTIVATE\_TYPE

Description, NML Type: -, 127

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\]

### EMC\_AXIS\_DEACTIVATE\_TYPE

Description, NML Type: -, 128

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\]

### EMC\_AXIS\_OVERRIDE\_LIMITS\_TYPE

Description, NML Type: overrides min/max limits during homing, 129

Obs: used from tkemc & mini through emc\_override\_limit

Written From: sendOverrideLimits (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcAxisOverrideLimits (taskintf.cc)
 which sends EMCMOT\_OVERRIDE\_LIMITS

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\]

### EMC\_AXIS\_SET\_OUTPUT\_TYPE

Description, NML Type: sets an DAC output value, 130

Obs: currently not used in EMC2, needs to go to HAL

Written From: sendAxisSetOutput (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcAxisSetOutput (taskintf.cc)
 which sends EMCMOT\_DAC\_OUT

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[output, double\]

### EMC\_AXIS\_LOAD\_COMP\_TYPE

Description, NML Type: loads compensation values from a file, 131

Obs: currently usrmotLoadComp if 0’ed in EMC2

Written From: sendAxisLoadComp (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcAxisLoadComp (minimill|bridgeporttaskintf.cc)
 which calls usrmotLoadComp

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[file, char\[LINELEN\]\]

### EMC\_AXIS\_ALTER\_TYPE

Description, NML Type: loads the alter value to modify the axis position, 132

Written From: sendAxisAlter (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcAxisAlter (taskintf.cc)
 which calls usrmotAlter

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[alter, double\]

### EMC\_AXIS\_SET\_STEP\_PARAMS\_TYPE

Description, NML Type: was used to set step related params, 133

Obs: currently not used in EMC2, needs to go to HAL
 (maybe directly from the ini, not through NML)

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcAxisSetStepParams (taskintf.cc)
 which sends EMCMOT\_SET\_STEP\_PARAMS

Parameter, Type: \[axis (in EMC\_AXIS\_CMD\_MSG), int\] \[setup\_time, double\] \[hold\_time, double\]

### EMC\_AXIS\_STAT\_TYPE

Description, NML Type: status for axis, not sent as a message but used as is, 199

Written From: none

Read To: none

Parameter, Type: \[a HUGE load of params\]

EMC TRAJ
--------

### EMC\_TRAJ\_SET\_AXES\_TYPE

Description, NML Type: -, 201

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[axes, int\]

### EMC\_TRAJ\_SET\_UNITS\_TYPE

Description, NML Type: -, 202

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[linearUnits, double\] \[angularUnits, double\]

### EMC\_TRAJ\_SET\_CYCLE\_TIME\_TYPE

Description, NML Type: -, 203

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[cycleTime, double\]

### EMC\_TRAJ\_SET\_MODE\_TYPE

Description, NML Type: -, 204

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[mode, enum EMC\_TRAJ\_MODE\_ENUM\]

### EMC\_TRAJ\_SET\_VELOCITY\_TYPE

Description, NML Type: sends a request to set the vel, which is in internal units/sec, 205

Written From: sendVelMsg (emccanon.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcTrajSetVelocity (minimill | bridgeporttaskintf.cc)
 which sends EMCMOT\_SET\_VEL

Parameter, Type: \[velocity, double\]

### EMC\_TRAJ\_SET\_ACCELERATION\_TYPE

Description, NML Type: -, 206

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[acceleration, double\]

### EMC\_TRAJ\_SET\_MAX\_VELOCITY\_TYPE

Description, NML Type: -, 207

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[velocity, double\]

### EMC\_TRAJ\_SET\_MAX\_ACCELERATION\_TYPE

Description, NML Type: -, 208

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[acceleration, double\]

### EMC\_TRAJ\_SET\_SCALE\_TYPE

Description, NML Type: set the feed override to be the percent value, 209

Obs: used for feed override messages

Written From: sendFeedOverride (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcTrajSetScale (taskintf.cc)
 which sends EMCMOT\_SCALE

Parameter, Type: \[scale, double\]

### EMC\_TRAJ\_SET\_MOTION\_ID\_TYPE

Description, NML Type: -, 210

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[id, int\]

### EMC\_TRAJ\_INIT\_TYPE

Description, NML Type: -, 211

Obs: not used

Written From: none

Read To: none

Parameter, Type:

### EMC\_TRAJ\_HALT\_TYPE

Description, NML Type: -, 212

Obs: not used

Written From: none

Read To: none

Parameter, Type:

### EMC\_TRAJ\_ENABLE\_TYPE

Description, NML Type: -, 213

Obs: not used

Written From: none

Read To: none

Parameter, Type:

### EMC\_TRAJ\_DISABLE\_TYPE

Description, NML Type: -, 214

Obs: not used

Written From: none

Read To: none

Parameter, Type:

### EMC\_TRAJ\_ABORT\_TYPE

Description, NML Type: causes traj to abort ?, 215

Obs: not used, only read

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcTrajAbort (taskintf.cc)
 which sends EMCMOT\_ABORT

Parameter, Type:

### EMC\_TRAJ\_PAUSE\_TYPE

Description, NML Type: causes traj to pause ?, 216

Obs: not used, only read

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcTrajPause (minimill | bridgeporttaskintf.cc)
 which sends EMCMOT\_PAUSE

Parameter, Type:

### EMC\_TRAJ\_STEP\_TYPE

Description, NML Type: -, 217

Obs: not used

Written From: none

Read To: none

Parameter, Type:

### EMC\_TRAJ\_RESUME\_TYPE

Description, NML Type: causes traj to resume ?, 218

Obs: not used, only read

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcTrajResume (minimill | bridgeporttaskintf.cc)
 which sends EMCMOT\_RESUME

Parameter, Type:

### EMC\_TRAJ\_DELAY\_TYPE

Description, NML Type: sets a delay in the task execution, 219

Obs: used with dwelling

Written From: DWELL (emccanon.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)

Parameter, Type: \[delay, double\]

### EMC\_TRAJ\_LINEAR\_MOVE\_TYPE

Description, NML Type: sends a linear move from the interp to motion, 220

Obs: used

Written From: STRAIGHT\_TRAVERSE, ARC\_FEED (emccanon.cc)

Read To: checkInterpList, emcTaskIssueCommand (emctaskmain.cc)
 calls emcTrajLinearMove (minimill | bridgeporttaskintf.cc)
 which sends EMCMOT\_SET\_LINE

Parameter, Type: \[end, EmcPose\]

### EMC\_TRAJ\_CIRCULAR\_MOVE\_TYPE

Description, NML Type: sends a circular move from the interp to motion, 221

Obs: used

Written From: ARC\_FEED (emccanon.cc)

Read To: checkInterpList, emcTaskIssueCommand (emctaskmain.cc)
 calls emcTrajCircularMove (minimill | bridgeporttaskintf.cc)
 which sends EMCMOT\_SET\_CIRCLE

Parameter, Type: \[end, EmcPose\] \[center, PM\_CARTESIAN\] \[normal, PM\_CARTESIAN\] \[turn, int\]

### EMC\_TRAJ\_SET\_TERM\_COND\_TYPE

Description, NML Type: chooses between blending or exact path mode, 222

Obs: used, seems the interp knows exact PATH, STOP and BLEND, motion however knows only BLEND or STOP

Written From: SET\_MOTION\_CONTROL\_MODE (emccanon.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcTrajSetTermCond (minimill | bridgeporttaskintf.cc)
 which sends EMCMOT\_TERM\_COND\_STOP or EMCMOT\_TERM\_COND\_BLEND

Parameter, Type: \[cond, int\]

### EMC\_TRAJ\_SET\_OFFSET\_TYPE

Description, NML Type: is used for tool length offset, 223

Obs: used, the message could transport more than just Z offset used for tool length

Written From: USE\_TOOL\_LENGTH\_OFFSET (emccanon.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 \` \`remembers the origin offset into emcStatus-&gt;task.origin

Parameter, Type: \[offset, EmcPose\]

### EMC\_TRAJ\_SET\_ORIGIN\_TYPE

Description, NML Type: sets the origin coords ?, 224

Obs: used

Written From: SET\_ORIGIN\_OFFSETS (emccanon.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 remembers the tool length offset

Parameter, Type: \[origin, EmcPose\]

### EMC\_TRAJ\_SET\_HOME\_TYPE

Description, NML Type: -, 225

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[home, EmcPose\]

### EMC\_TRAJ\_SET\_PROBE\_INDEX\_TYPE

Description, NML Type: sends the index pin used for probing, 226

Obs: should get obsolete, probin pin should get routed by HAL

Written From: sendSetProbeIndex (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcTrajSetProbeIndex (minimill | bridgeporttaskintf.cc)
 which sends EMCMOT\_SET\_PROBE\_INDEX

Parameter, Type: \[index, int\]

### EMC\_TRAJ\_SET\_PROBE\_POLARITY\_TYPE

Description, NML Type: sends the polarity for the pin used for probing, 227

Obs: should get obsolete, probin pin polarity should get routed by HAL

Written From: sendSetProbePolarity (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcTrajSetProbePolarity (minimill | bridgeporttaskintf.cc)
 which sends EMCMOT\_SET\_PROBE\_POLARITY

Parameter, Type: \[polarity, int\]

### EMC\_TRAJ\_CLEAR\_PROBE\_TRIPPED\_FLAG\_TYPE

Description, NML Type: clears the probe tripped, 228

Obs: used

Written From: TURN\_PROBE\_ON (emccanon.cc)
 sendClearProbeTrippedFlag (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcTrajClearProbeTrippedFlag (minimill | bridgeporttaskintf.cc)
 which sends EMCMOT\_CLEAR\_PROBE\_FLAGS

Parameter, Type:

### EMC\_TRAJ\_PROBE\_TYPE

Description, NML Type: performs a straight probe move, 229

Obs: used

Written From: STRAIGHT\_PROBE (emccanon.cc) sendProbe (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcTrajProbe (minimill | bridgeporttaskintf.cc)
 which sends EMCMOT\_PROBE

Parameter, Type: \[pos, EmcPose\]

### EMC\_TRAJ\_SET\_TELEOP\_ENABLE\_TYPE

Description, NML Type: sets the traj mode to teleop, 230

Obs: used

Written From: sendSetTeleopEnable (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcTrajSetMode (minimill | bridgeporttaskintf.cc)
 which sends EMCMOT\_TELEOP

Parameter, Type: \[enable, int\]

### EMC\_TRAJ\_SET\_TELEOP\_VECTOR\_TYPE

Description, NML Type: jogs in teleop mode, 231

Obs: used for jogging in teleop mode

Written From: sendJogCont (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcTrajSetTeleopVector (minimill | bridgeporttaskintf.cc)
 which sends EMCMOT\_SET\_TELEOP\_VECTOR

Parameter, Type: \[vector, EmcPose\]

### EMC\_TRAJ\_STAT\_TYPE

Description, NML Type: status for traj, not sent as a message but used as is, 299

Written From: none

Read To: none

Parameter, Type: \[a HUGE load of params\]

EMC MOTION
----------

### EMC\_MOTION\_INIT\_TYPE

Description, NML Type: -, 301

Obs: not used

Written From: none

Read To: none

Parameter, Type:

### EMC\_MOTION\_HALT\_TYPE

Description, NML Type: -, 302

Obs: not used

Written From: none

Read To: none

Parameter, Type:

### EMC\_MOTION\_ABORT\_TYPE

Description, NML Type: -, 303

Obs: not used

Written From: none

Read To: none

Parameter, Type:

### EMC\_MOTION\_SET\_AOUT\_TYPE

Description, NML Type: sets an analog output value coordinated with motion, 304

Obs: emccanon.cc currently lacks this in EMC2, not used in EMC2, needs to go to HAL

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcMotionSetAout (minimill | bridgeporttaskintf.cc)
 which sends EMCMOT\_SET\_AOUT

Parameter, Type: \[index, unsigned char\] \[start, double\] \[end, double\] \[now, unsigned char\]

### EMC\_MOTION\_SET\_DOUT\_TYPE

Description, NML Type: sets an digital output value coordinated with motion, 305

Obs: emccanon.cc currently lacks this in EMC2, not used in EMC2, needs to go to HAL

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcMotionSetDout (minimill | bridgeporttaskintf.cc)
 which sends EMCMOT\_SET\_DOUT

Parameter, Type: \[index, unsigned char\] \[start, double\] \[end, double\]

### EMC\_MOTION\_STAT\_TYPE

Description, NML Type: status for motion, not sent as a message but used as is, 399

Written From: none

Read To: none

Parameter, Type: \[heartbeat, unsigned long int\] \[traj, EMC\_TRAJ\_STAT\] \[axis\[EMC\_AXIS\_MAX\], EMC\_AXIS\_STAT\]

EMC TASK
--------

### EMC\_TASK\_INIT\_TYPE

Description, NML Type: calls the Task init(), 501

Obs: not used, emcTaskInit called directly from emctask\_startup()

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc) calls emcTaskInit()

Parameter, Type:

### EMC\_TASK\_HALT\_TYPE

Description, NML Type: -, 502

Written From: none

Read To: none

Parameter, Type:

### EMC\_TASK\_ABORT\_TYPE

Description, NML Type: aborts task, cleans up, 503

Obs: used on shutdown

Written From: sendAbort (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc) aborts all

Parameter, Type:

### EMC\_TASK\_SET\_MODE\_TYPE

Description, NML Type: sets current TASK mode, MANUAL, MDI, AUTO, 504

Obs: used for switching the current mode

Written From: sendManual sendMdi sendAuto (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcTaskSetMode (emcTask.cc)

Parameter, Type: \[mode, enum EMC\_TASK\_MODE\_ENUM\]

### EMC\_TASK\_SET\_STATE\_TYPE

Description, NML Type: , 505

Written From: none

Read To: none

Parameter, Type: \[state, enum EMC\_TASK\_STATE\_ENUM\]

### EMC\_TASK\_PLAN\_OPEN\_TYPE

Description, NML Type: , 506

Written From: none

Read To: none

Parameter, Type: \[file, char\[LINELEN\]\]

### EMC\_TASK\_PLAN\_RUN\_TYPE

Description, NML Type: , 507

Written From: none

Read To: none

Parameter, Type: \[line, int\]

### EMC\_TASK\_PLAN\_READ\_TYPE

Description, NML Type: , 508

Written From: none

Read To: none

Parameter, Type:

### EMC\_TASK\_PLAN\_EXECUTE\_TYPE

Description, NML Type: , 509

Written From: none

Read To: none

Parameter, Type: \[command, char\[LINELEN\]\]

### EMC\_TASK\_PLAN\_PAUSE\_TYPE

Description, NML Type: , 510

Written From: none

Read To: none

Parameter, Type:

### EMC\_TASK\_PLAN\_STEP\_TYPE

Description, NML Type: , 511

Written From: none

Read To: none

Parameter, Type:

### EMC\_TASK\_PLAN\_RESUME\_TYPE

Description, NML Type: , 512

Written From: none

Read To: none

Parameter, Type:

### EMC\_TASK\_PLAN\_END\_TYPE

Description, NML Type: , 513

Written From: none

Read To: none

Parameter, Type:

### EMC\_TASK\_PLAN\_CLOSE\_TYPE

Description, NML Type: , 514

Written From: none

Read To: none

Parameter, Type:

### EMC\_TASK\_PLAN\_INIT\_TYPE

Description, NML Type: , 515

Written From: none

Read To: none

Parameter, Type:

### EMC\_TASK\_PLAN\_SYNCH\_TYPE

Description, NML Type: , 516

Written From: none

Read To: none

Parameter, Type:

### EMC\_TASK\_STAT\_TYPE

Description, NML Type: , 599

Written From: none

Read To: none

Parameter, Type: \[heartbeat, unsigned long int\] \[a HUGE load of params\]

EMC TOOL
--------

### EMC\_TOOL\_INIT\_TYPE

Description, NML Type: starts TOOL init, 1101

Obs: used for initializing the IO stuff, should load the tool table too

Written From: emcIoInit (iotaskintf.cc)

Read To: main (ioControl.cc simIoControl.cc)
 emc\_io\_get\_command (iosh.cc)

Parameter, Type:

### EMC\_TOOL\_HALT\_TYPE

Description, NML Type: stops TOOL, 1102

Obs: used for stopping IO, doesn’t actually do anything so far, in EMC1 it was send to subordinates too (spindle, aux, coolant, lube)

Written From: emcIoHalt (iotaskintf.cc)

Read To: main (ioControl.cc simIoControl.cc)
 emc\_io\_get\_command (iosh.cc)

Parameter, Type:

### EMC\_TOOL\_ABORT\_TYPE

Description, NML Type: aborts TOOL, 1103

Obs: used for aborting IO, doesn’t actually do anything so far, in EMC1 it was send to subordinates too (spindle, aux, coolant, lube)

Written From: emcIoAbort (iotaskintf.cc)

Read To: main (ioControl.cc simIoControl.cc)
 emc\_io\_get\_command (iosh.cc)

Parameter, Type:

### EMC\_TOOL\_PREPARE\_TYPE

Description, NML Type: prepares a tool for tool changing, 1104

Obs: loads the prep tool in emcioStatus.tool.toolPrepped, should go to PLC and make it move the desired tool in the toolchanging position

Written From: SELECT\_TOOL (emccanon.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcToolPrepare (iotaskintf.cc)
 which sends it to the IO controller

Parameter, Type: \[tool, int\]

### EMC\_TOOL\_LOAD\_TYPE

Description, NML Type: changes the current tool with the prepared tool, 1105

Obs: loads the actual tool, makes toolprepped=0

Written From: CHANGE\_TOOL (emccanon.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcToolLoad (iotaskintf.cc)
 which sends it to the IO controller,
 main (simIoControl.cc ioControl.cc)

Parameter, Type:

### EMC\_TOOL\_UNLOAD\_TYPE

Description, NML Type: unloads the current tool from the spindle, 1106

Obs: unloads the current tool, not written in EMC2 only read

Written From: none

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcToolUnLoad (iotaskintf.cc)
 which sends it to the IO controller,
 main (simIoControl.cc ioControl.cc)

Parameter, Type:

### EMC\_TOOL\_LOAD\_TOOL\_TABLE\_TYPE

Description, NML Type: loads the tool table, without this tool comp. can’t be made, 1107

Written From: sendLoadToolTable (emcsh.cc)

Read To: none

Parameter, Type: \[file, char\[LINELEN\]\]

### EMC\_TOOL\_SET\_OFFSET\_TYPE

Description, NML Type: , 1108

Written From: none

Read To: none

Parameter, Type: \[tool, int\] \[length, double\] \[diameter, double\]

### EMC\_TOOL\_STAT\_TYPE

Description, NML Type: , 1199

Written From: none

Read To: none

Parameter, Type:

EMC AUX
-------

### EMC\_AUX\_INIT\_TYPE

Description, NML Type: , 1201

Written From: none

Read To: none

Parameter, Type:

### EMC\_AUX\_HALT\_TYPE

Description, NML Type: , 1202

Written From: none

Read To: none

Parameter, Type:

### EMC\_AUX\_ABORT\_TYPE

Description, NML Type: , 1203

Written From: none

Read To: none

Parameter, Type:

### EMC\_AUX\_DIO\_WRITE\_TYPE

Description, NML Type: , 1204

Written From: none

Read To: none

Parameter, Type: \[index, int\] \[value, int\]

### EMC\_AUX\_AIO\_WRITE\_TYPE

Description, NML Type: , 1205

Written From: none

Read To: none

Parameter, Type: \[index, int\] \[value, double\]

### EMC\_AUX\_ESTOP\_ON\_TYPE

Description, NML Type: , 1206

Written From: none

Read To: none

Parameter, Type:

### EMC\_AUX\_ESTOP\_OFF\_TYPE

Description, NML Type: , 1207

Written From: none

Read To: none

Parameter, Type:

### EMC\_AUX\_STAT\_TYPE

Description, NML Type: , 1299

Written From: none

Read To: none

Parameter, Type: \[estop, int\] \[estopIn, int\] \[dout, unsigned char\[EMC\_AUX\_MAX\_DOUT\]\] \[din, unsigned char\[EMC\_AUX\_MAX\_DIN\]\] \[aout, double\[EMC\_AUX\_MAX\_AOUT\]\] \[ain, double\[EMC\_AUX\_MAX\_AIN\]\]

EMC SPINDLE
-----------

### EMC\_SPINDLE\_INIT\_TYPE

Description, NML Type: , 1301

Written From: none

Read To: none

Parameter, Type:

### EMC\_SPINDLE\_HALT\_TYPE

Description, NML Type: , 1302

Written From: none

Read To: none

Parameter, Type:

### EMC\_SPINDLE\_ABORT\_TYPE

Description, NML Type: , 1303

Written From: none

Read To: none

Parameter, Type:

### EMC\_SPINDLE\_ON\_TYPE

Description, NML Type: , 1304

Written From: none

Read To: none

Parameter, Type: \[speed, double\]

### EMC\_SPINDLE\_OFF\_TYPE

Description, NML Type: , 1305

Written From: none

Read To: none

Parameter, Type:

### EMC\_SPINDLE\_FORWARD\_TYPE

Description, NML Type: , 1306

Written From: none

Read To: none

Parameter, Type: \[speed, double\]

### EMC\_SPINDLE\_REVERSE\_TYPE

Description, NML Type: , 1307

Written From: none

Read To: none

Parameter, Type: \[speed, double\]

### EMC\_SPINDLE\_STOP\_TYPE

Description, NML Type: , 1308

Written From: none

Read To: none

Parameter, Type: \[speed, double\]

### EMC\_SPINDLE\_INCREASE\_TYPE

Description, NML Type: , 1309

Written From: none

Read To: none

Parameter, Type: \[speed, double\]

### EMC\_SPINDLE\_DECREASE\_TYPE

Description, NML Type: , 1310

Written From: none

Read To: none

Parameter, Type:

### EMC\_SPINDLE\_CONSTANT\_TYPE

Description, NML Type: , 1311

Written From: none

Read To: none

Parameter, Type:

### EMC\_SPINDLE\_BRAKE\_RELEASE\_TYPE

Description, NML Type: , 1312

Written From: none

Read To: none

Parameter, Type:

### EMC\_SPINDLE\_BRAKE\_ENGAGE\_TYPE

Description, NML Type: , 1313

Written From: none

Read To: none

Parameter, Type:

### EMC\_SPINDLE\_ENABLE\_TYPE

Description, NML Type: , 1314

Written From: none

Read To: none

Parameter, Type:

### EMC\_SPINDLE\_DISABLE\_TYPE

Description, NML Type: , 1315

Written From: none

Read To: none

Parameter, Type:

### EMC\_SPINDLE\_STAT\_TYPE

Description, NML Type: , 1399

Written From: none

Read To: none

Parameter, Type: \[speed, double\] \[direction, int\] \[brake, int\] \[increasing, int\] \[enabled, int\]

EMC COOLANT
-----------

### EMC\_COOLANT\_INIT\_TYPE

Description, NML Type: initializes the COOLANT controller (currently part of the IO controller), 1401

Obs: not written in EMC2, only read, in EMC1 it was sent when TOOL\_INIT was sent

Written From: none

Read To: main (ioControl.cc simIoControl.cc)
 emc\_io\_get\_command (iosh.cc)

Parameter, Type: none

### EMC\_COOLANT\_HALT\_TYPE

Description, NML Type: stops the COOLANT, 1402

Obs: not written in EMC2, only read, in EMC1 it was sent when TOOL\_HALT was sent

Written From: none

Read To: main (ioControl.cc simIoControl.cc)
 emc\_io\_get\_command (iosh.cc)

Parameter, Type: none

### EMC\_COOLANT\_ABORT\_TYPE

Description, NML Type: aborts the COOLANT, 1403

Obs: not written in EMC2, only read, in EMC1 it was sent when TOOL\_ABORT was sent

Written From: none

Read To: main (ioControl.cc simIoControl.cc)
 emc\_io\_get\_command (iosh.cc)

Parameter, Type: none

### EMC\_COOLANT\_MIST\_ON\_TYPE

Description, NML Type: starts MIST coolant, 1404

Obs: used, written by emccanon.cc

Written From: MIST\_ON (emccanon.cc) sendMistOn (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcCoolantMistOn (iotaskintf.cc)
 which sends it to the IO controller,
 main (simIoControl.cc ioControl.cc) iosh.cc

Parameter, Type: none

### EMC\_COOLANT\_MIST\_OFF\_TYPE

Description, NML Type: stops MIST coolant, 1405

Obs: used, written by emccanon.cc

Written From: MIST\_OFF (emccanon.cc) sendMistOff (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcCoolantMistOff (iotaskintf.cc)
 which sends it to the IO controller,
 main (simIoControl.cc ioControl.cc) iosh.cc

Parameter, Type: none

### EMC\_COOLANT\_FLOOD\_ON\_TYPE

Description, NML Type: starts FLOOD coolant, 1406

Obs: used, written by emccanon.cc

Written From: FLOOD\_ON (emccanon.cc) sendFloodOn (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcCoolantFloodOn (iotaskintf.cc)
 which sends it to the IO controller,
 main (simIoControl.cc ioControl.cc) iosh.cc

Parameter, Type: none

### EMC\_COOLANT\_FLOOD\_OFF\_TYPE

Description, NML Type: stops FLOOD coolant, 1407

Obs: used, written by emccanon.cc

Written From: FLOOD\_OFF (emccanon.cc) sendFloodOff (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcCoolantFloodOff (iotaskintf.cc)
 which sends it to the IO controller,
 main (simIoControl.cc ioControl.cc) iosh.cc

Parameter, Type: none

### EMC\_COOLANT\_STAT\_TYPE

Description, NML Type: status for coolant, not sent as a message but used as is, 1499

Written From: none

Read To: none

Parameter, Type: \[mist, int\] \[flood, int\]

EMC LUBE
--------

### EMC\_LUBE\_INIT\_TYPE

Description, NML Type: initializes the LUBE controller (currently part of the IO controller), 1501

Obs: not written in EMC2, only read, in EMC1 it was sent when TOOL\_INIT was sent

Written From: none

Read To: main (ioControl.cc simIoControl.cc)
 emc\_io\_get\_command (iosh.cc)

Parameter, Type: none

### EMC\_LUBE\_HALT\_TYPE

Description, NML Type: stops the LUBE, 1502

Obs: not written in EMC2, only read, in EMC1 it was sent when TOOL\_HALT was sent

Written From: none

Read To: main (ioControl.cc simIoControl.cc)
 emc\_io\_get\_command (iosh.cc)

Parameter, Type: none

### EMC\_LUBE\_ABORT\_TYPE

Description, NML Type: aborts the LUBE, 1503

Obs: not written in EMC2, only read, in EMC1 it was sent when TOOL\_ABORT was sent

Written From: none

Read To: main (ioControl.cc simIoControl.cc)
 emc\_io\_get\_command (iosh.cc)

Parameter, Type: none

### EMC\_LUBE\_ON\_TYPE

Description, NML Type: starts LUBE, 1504

Obs: written only by the GUIs (emcsh.cc)

Written From: sendLubeOn (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcLubeOn (iotaskintf.cc)
 which sends it to the IO controller,
 main (ioControl.cc simIoControl.cc)
 emc\_io\_get\_command (iosh.cc)

Parameter, Type: none

### EMC\_LUBE\_OFF\_TYPE

Description, NML Type: stops LUBE, 1505

Obs: written only by the GUIs (emcsh.cc)

Written

From: sendLubeOff (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcLubeOff (iotaskintf.cc)
 which sends it to the IO controller,
 main (ioControl.cc simIoControl.cc)
 emc\_io\_get\_command (iosh.cc)

Parameter, Type: none

### EMC\_LUBE\_STAT\_TYPE

Description, NML Type: status for LUBE, not sent as a message but used as is, 1599

Written From: none

Read To: none

Parameter, Type: \[on, int\] \[level, int\]

EMC SET
-------

### EMC\_SET\_DIO\_INDEX\_TYPE

Description, NML Type: , 5001

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[value, int\] \[index, int\]

### EMC\_SET\_AIO\_INDEX\_TYPE

Description, NML Type: , 5002

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[value, int\] \[index, int\]

### EMC\_SET\_POLARITY\_TYPE

Description, NML Type: , 5003

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[value, int\] \[polarity, int\]

EMC IO
------

### EMC\_IO\_INIT\_TYPE

Description, NML Type: , 1601

Obs: not written in EMC2, only read

Written From: none

Read To: main (ioControl.cc simIoControl.cc)
 emc\_io\_get\_command (iosh.cc)

Parameter, Type:

### EMC\_IO\_HALT\_TYPE

Description, NML Type: , 1602

Obs: not used

Written From: none

Read To: none

Parameter, Type:

### EMC\_IO\_ABORT\_TYPE

Description, NML Type: , 1603

Obs: not used

Written From: none

Read To: none

Parameter, Type:

### EMC\_IO\_SET\_CYCLE\_TIME\_TYPE

Description, NML Type: , 1604

Obs: not used

Written From: none

Read To: none

Parameter, Type: \[cycleTime, double\]

### EMC\_IO\_STAT\_TYPE

Description, NML Type: status for IO, not sent as a message but used as is, 1699

Written From: none

Read To: none

Parameter, Type: \[heartbeat, unsigned long int\] \[tool, EMC\_TOOL\_STAT\] \[spindle, EMC\_SPINDLE\_STAT\] \[coolant, EMC\_COOLANT\_STAT\] \[aux, EMC\_AUX\_STAT\] \[lube, EMC\_LUBE\_STAT\]

EMC INIT, HALT, & ABORT
-----------------------

### EMC\_INIT\_TYPE

Description, NML Type: , 1901

Obs: not used

Written From: none

Read To: none

Parameter, Type:

### EMC\_HALT\_TYPE

Description, NML Type: , 1902

Obs: not used

Written From: none

Read To: none

Parameter, Type:

### EMC\_ABORT\_TYPE

Description, NML Type: , 1903

Obs: not used

Written From: none

Read To: none

Parameter, Type:

EMC LOG
-------

### EMC\_LOG\_OPEN\_TYPE

Description, NML Type: opens the log file, 1904

Obs: not used in EMC2, it was used in EMC\[1\] from emclog.tcl

Written From: sendLogOpen (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcLogOpen (taskintf.cc)
 which sends EMCMOT\_OPEN\_LOG

Parameter, Type: \[file, char\[LINELEN\]\] \[type, int\] \[size, int\] \[skip, int\] \[which, int\] \[triggerType, int\] \[triggerVar, int\] \[triggerThreshold, double\]

### EMC\_LOG\_START\_TYPE

Description, NML Type: starts logging, 1905

Obs: not used in EMC2, it was used in EMC\[1\] from emclog.tcl

Written From: sendLogStart (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc) calls
 emcLogStart (taskintf.cc)
 which sends EMCMOT\_START\_LOG

Parameter, Type: none

### EMC\_LOG\_STOP\_TYPE

Description, NML Type: stops logging, 1906

Obs: not used in EMC2, it was used in EMC\[1\] from emclog.tcl

Written From: sendLogStop (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc) calls
 emcLogStop (taskintf.cc)
 which sends EMCMOT\_STOP\_LOG

Parameter, Type: none

### EMC\_LOG\_CLOSE\_TYPE

Description, NML Type: closes the log file, 1907

Obs: not used in EMC2, it was used in EMC\[1\] from emclog.tcl

Written From: sendLogClose (emcsh.cc)

Read To: emcTaskIssueCommand (emctaskmain.cc)
 calls emcLogClose (taskintf.cc)
 which sends EMCMOT\_CLOSE\_LOG

Parameter, Type: none

EMC STA T
---------

### EMC\_STAT\_TYPE

Description, NML Type: aggregation of all the status messages,
 not sent as a message but used as is all over the place, 1999

Written From: none

Read To: none

Parameter, Type: \[task, EMC\_TASK\_STAT\]
 \[motion, EMC\_MOTION\_STAT\]
 \[io, EMC\_IO\_STAT\]
 \[logFile, char\[LINELEN\]\]
 \[logType, int\]
 \[logSize, int\]
 \[logSkip, int\]
 logOpen, int\] \[logStarted, int\] \[logPoints, int\]

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


