== CmdArduino


CmdArduino is a small library to parse commands from the Serial port
or anything else which implements the Stream API.  It is based on the
original by Akiba at Freaklabs.org

To use it create your command functions, then wire them up in your setup function.
All command functions must take `int arg_cnt, char ** args` as it's parameters
and return `void`.
Here's an example from a simple robot:

```
void left(int arg_cnt, char **args) {
    LeftMotor->run(FORWARD);
    delay(200);
    LeftMotor->run(RELEASE);
}

void right(int arg_cnt, char **args) {
    RightMotor->run(FORWARD);
    delay(200);
    RightMotor->run(RELEASE);
}

void setup() {
    Serial.begin(9600);
    cmdInit(&Serial);
    cmdAdd('left',left);
    cmdAdd('right',right);
}
```

Call `cmdInit` with a pointer to the stream you are using. Call `cmdAdd`
to attach a string command to the command function.  Now you can type `left`
or `right` into your serial port to command the robot.  The serial port
will be parsed using newline, `\n`, as the delimiter. The command name should
be the first item and any arguments after that.  For example, to tell the robot
to turn left for three seconds, type in:

```
left 3000\n
```

Then the left function becomes

```
void left(int arg_cnt, char **args) {
    int time = 200;
    if(arg_cnt > 0) {
        time = parseNum(args[0]);
    }
    LeftMotor->run(FORWARD);
    delay(time);
    LeftMotor->run(RELEASE);
}
```







== original readme

The Arduino Command Line Interface, aka CmdArduino, is a simple shell that can
be run on an Arduino. It's nothing fancy and its main purpose is to allow users
to easily call their functions on a running Arduino via a simple serial
terminal. It also allows users to pass in arguments from the command line into
the functions they wrote so they can easily toggle pins, set blinking speed,
set pwm duty cycles, or whatever else might need command line user input. Using
it is fairly simple and just requires unzipping the files into the
"Arduino/libraries" sub-directory in the Arduino program folder.
