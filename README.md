# YamShell (Yet Another Mbed Shell)
 Library to add interactive shell with your own commands in easy to use way in Mbed OS 6. Has feature to preserve the input you have currently typed! This makes it easier to have some output functions going and still send commands.

# Installation
Mbed Studio:
- File (from top bar)
- Add library 
- put in `https://github.com/SuperThunder/YamShell.git`

If Mbed studio throws an error, use:
- Terminal (from top bar)
- Use the command for Mbed CLI

Mbed CLI: `mbed add https://github.com/SuperThunder/YamShell.git`

In mbed_app.json, you will likely want
```json
...
"target.c_lib": "std",
"target.printf_lib": "minimal", 
"platform.stdio-buffered-serial": 1, 
"platform.stdio-baud-rate": 115200
...
```
If you need completely standard printf/scanf, then printf_lib should be set to std.

# Usage
Recommended terminal is MobaXTerm or similar (Not the Mbed Studio terminal, close it and open the terminal in MobaXTerm).
```C++
//main.cpp
//Create a YamShell object at the top of your program (in main.cpp)
YamShell ys(USBTX, USBRX, 115200);

//Registers your command callbacks
//callbacks must have signature void(int argc, char** argv)
ys.registerCommand("mycommand", &my_command_function);
ys.registerCommand("myclasscommand", callback(&MyObj, MyClass::MyClassFunction));

//Use any of the output functions:
// void write(const void *buf, std::size_t len);
// void print(const char* s);
// void println(const char* s);
// void printf(const char fmt[], ...);
// eg,
ys.println("Hello!");
ys.printf("Current reading: %f\n", adc_reading*3.3);
```

Example callback:
```C++
void command_duty(int argc, char** argv)
{
    //just command name
    if(argc == 1)
    {
        ys.printf("PWM duty: %f\n", config.pwm_duty);
    }
    //if value provided too
    else if(argc == 2)
    {
        float tmp_f;
        int status = sscanf(argv[1], "%f", &tmp_f);
        //set duty
        config.pwm_duty = tmp_f;
        fan_pwm.write(tmp_f);
    }
}
```


Anywhere in your code can print to the CLI as long as it can access the YamShell object.


## EventQueue
As shown in the [Mbed docs](https://os.mbed.com/docs/mbed-os/v6.15/apis/scheduling-tutorials.html), EventQueues can be used to move the execution of printf from ISR or high performance context (printf not allowed and undesirable anyway) to an EventQueue thread. This can be used with the YamShell functions, except for printf, as Mbed Callback does not support variadic member functions.

# Example
Example program using YamShell to provide an interface for a PWM fan interface [here](https://github.com/SuperThunder/Mbed-OS6-4PinFanPWM/blob/master/main.cpp).

# What are the other Mbed shells?
- Natural Tiny Shell port: https://iotexpert.com/ntshell-for-mbed-os/ and https://github.com/iotexpert/middleware-ntshell. The real deal, but harder to use than just ys.register(), and may need some changes for Mbed OS6.
- A sort of semi official but mentioned nowhere else shell: https://github.com/PelionIoT/mbed-client-cli
- Various shells (mainly for Mbed 2 / OS5) all over the Mbed code repository and Github