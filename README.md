# Recruits Task - Week #2
- ## [git para noobs](https://hackmd.io/@PedroRomao/HJ0GJSae1x)
- ## Add this file to your local git repository create a new branch and work on it, when you are done or as you complete the questions merge the branch into the main branch (remote main branch will have your final work, this means main branch on GitHub, please make the repository public for the next weeks).
- ## Perform the following tasks to the best of your hability, sometimes, in the questions, there are multiple answers just tell us what you think, feel free to use the group to ask questions.

### 1
**1.** Check out the [el-sw repository](https://github.com/fs-feup/el-sw/tree/main) code and documentation  and try to generally understand what the software does in each device (there is no need to understand all the little details).
### 2
When we read values from the brake sensor (C1) and the apps (C3) we do not use the most recent reading and use instead a different approach. Explain the approach and why you think it is used.

**Answer:** Values go through a moving average before being sent. We use this aproach to reduce error from physicly vibrating the atuators.


### 3
Check out the R2D(Ready To Drive) code on the C3 state machine. In the condition below we use a timer (R2DTimer) to check the brake was engaged instead of just checking the brake pressure received from can, why?
```c++
        if ((r2dButton.fell() and TSOn and R2DTimer < R2D_TIMEOUT) or R2DOverride)
        {
            playR2DSound();
            initBamocarD3();
            request_dataLOG_messages();
            R2DStatus = DRIVING;
            break;
        }
```

**Answer:** We use that timer to remove bad timing. So that, when commiting to the R2D mode, it must be on porpouse. 
### 4
What is the ID of the can message sent to the bamocar to request torque?
**Answer:** 0x181
### 5 
The code below is not amazing, tell us some things you would change to improve it, you can write them down in text or correct the code:
```c++
// this is a class for my car
class mycar {
private:
    int sensor_reading1; // hydraulic pressure sensor
    int sensor_reading2; // temperature sensor
    int sensor_reading3; // humidity sensor
    int sensor_reading4; // light sensor
    int sensor_reading5; // sound sensor
    int sensor_reading6; // distance sensor
    int sensor_reading7; // accelerometer sensor
    int sensor_reading8; // gyroscope sensor

    int sensor_reading9; // old sensor, not used anymore

public:
    mycar() : sensor_reading1(0), sensor_reading2(0), sensor_reading3(0), sensor_reading4(0),
            sensor_reading5(0), sensor_reading6(0), sensor_reading7(0), sensor_reading8(0) {}

    // Method will update readings by analog reading and print them 
    void updateprint() {
        sensor_reading1 = analogRead(0); // pin 0 is connected to the hydraulic pressure sensor
        sensor_reading2 = analogRead(1); // pin 1 is connected to the temperature sensor
        sensor_reading3 = analogRead(2); // pin 2 is connected to the humidity sensor
        sensor_reading4 = analogRead(3); // pin 3 is connected to the light sensor
        sensor_reading5 = analogRead(4); // pin 4 is connected to the sound sensor
        sensor_reading6 = analogRead(5); // pin 5 is connected to the distance sensor
        sensor_reading7 = analogRead(6); // pin 6 is connected to the accelerometer sensor
        sensor_reading8 = analogRead(7); // pin 7 is connected to the gyroscope sensor
        func(sensor_reading1, sensor_reading2, sensor_reading3, sensor_reading4, 
              sensor_reading5, sensor_reading6, sensor_reading7, sensor_reading8);// print the readings
    }

    // function to print the readings of the sensors
    void func(int sensor_reading1, int sensor_reading2, int sensor_reading3, int sensor_reading4, 
              int sensor_reading5, int sensor_reading6, int sensor_reading7, int sensor_reading8) {
        Serial.print("Sensor Reading 1: "); Serial.println(sensor_reading1);
        Serial.print("Sensor Reading 2: "); Serial.println(sensor_reading2);
        Serial.print("Sensor Reading 3: "); Serial.println(sensor_reading3);
        Serial.print("Sensor Reading 4: "); Serial.println(sensor_reading4);
        Serial.print("Sensor Reading 5: "); Serial.println(sensor_reading5);
        Serial.print("Sensor Reading 6: "); Serial.println(sensor_reading6);
        Serial.print("Sensor Reading 7: "); Serial.println(sensor_reading7);
        Serial.print("Sensor Reading 8: "); Serial.println(sensor_reading8);
        //all readings were serial printed
    }
};
```
**Answer:** Firstly remove any varible that is not being use, in this case "sensor_reading9". Secondly reduce the amount of text present, use vectores instead of a variable that only changes a number. Lastly modify the function to take advantage of the vector.

This is the code I tried to develop:

´´´c++

// this is a class for my car
class mycar {
private:
    std::vector<int> sensor_reading();

public:
    mycar() :
            sensor_reading(8,0){}
        
    void updateprint() 
    {
        for (int i=0; i<sensor_reading.size(); i++)
        {
            sensor_reading[i] = analogRead(i);
            func(sensor_reading(i));
        }
    }

    void func(sensor_reading())
        {
            for (int i = 0; i < sensor_reading.size(); i++)
            {
                Serial.print("Sensor Reading %d: %d\n", i + 1, sensor_reading[i]); 
            }
        }
}
```

