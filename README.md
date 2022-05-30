# YamShell (Yet Another Mbed Shell)
 Library to add interactive shell with your own commands in easy to use way in Mbed OS 6. Has feature to preserve the input you have currently typed! This makes it easier to have some output functions going and still send commands.

# Usage
Recommended terminal is MobaXTerm or similar (Not the Mbed Studio terminal, close it and open the terminal in MobaXTerm).
```C++
//main.cpp
//Create a YamShell object at the top of your program (in main.cpp)
YamShell ys(USBTX, USBRX, 115200);

//Registers your command callbacks
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

Anywhere in your code can print to the CLI as long as it can access the YamShell object.

# Example
Example program using YamShell to provide an interface for a PWM fan interface [here](https://github.com/SuperThunder/Mbed-OS6-4PinFanPWM/blob/master/main.cpp).

# What are the other Mbed shells?
- Natural Tiny Shell port: https://iotexpert.com/ntshell-for-mbed-os/ and https://github.com/iotexpert/middleware-ntshell. The real deal, but harder to use than just ys.register(), and may need some changes for Mbed OS6.
- A sort of semi official but documented nowhere shell: https://github.com/PelionIoT/mbed-client-cli
- Various shells (mainly for Mbed 2 / OS5) all over the Mbed code repository and Github