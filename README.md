%Drawing arm

clc;
clear;
close all;

a = arduino('COM2','Mega2560','Libraries','Adafruit\MotorShieldV2');
shield1 = addon(a,'Adafruit\MotorShieldV2');

writeDigitalPin(a,'D22',1);
writeDigitalPin(a,'D23',0);
writeDigitalPin(a,'D24',1);
writeDigitalPin(a,'D25',0);
writeDigitalPin(a,'D26',1);
writeDigitalPin(a,'D27',0);
writeDigitalPin(a,'D28',1);
writeDigitalPin(a,'D29',0);

%Motor setup
M1 = dcmotor(shield1,1);
M2 = dcmotor(shield1,2);
M3 = dcmotor(shield1,3);
M4 = dcmotor(shield1,4);

M1Degree = 360*(readVoltage(a, 'A8')/5);
M2Degree = 360*(readVoltage(a, 'A9')/5);
M3Degree = 360*(readVoltage(a, 'A10')/5);
M4Degree = 360*(readVoltage(a, 'A11')/5);

p1 = [ 7, 7, 5];

M1FirstStart = 0;%Start motor only when needed
M2FirstStart = 0;
M3FirstStart = 0;
currentAngles = [0,0,0];
speeds = [0,0,0];
firstRun  = 0;
%Move to specified angles
j = size(points);
while (M1Degree < p1(1,0) || M1Degree > p1(1,0) || M2Degree < p1(1,1) || M2Degree > p2(1,1) || M3Degree < p3(1,2) || M3Degree > p3(1,2))

        %Motor1
        if (M1Degree < p1(1,0))
            if(M1FirstStart == 0)
                start(M1);
                M1FirstStart = M1FirstStart+1;
            end
            M1.Speed = -0.8;
        elseif(M1Degree > p1(1,0))
            if(M1FirstStart == 0)
                start(M1);
                M1FirstStart = M1FirstStart+1;
            end
            M1.Speed = 0.8;
        else
            stop(M1);
        end
        M1P = readVoltage(a, 'A8');
        M1Degree = 360*(M1P/5);
        
        %Motor2
        if (M2Degree < p1(1,1))
            if(M2FirstStart == 0)
                start(M2);
                M2FirstStart = M2FirstStart+1;
            end
            M2.Speed = -0.2;%-0.2;%
        elseif (M2Degree > p1(1,1))
            if(M2FirstStart == 0)
                start(M2);
                M2FirstStart = M2FirstStart+1;
            end
            M2.Speed = 0.2;%0.2;
        else
            stop(M2);
        end
        M2P = readVoltage(a, 'A9');
        M2Degree = 360*(M2P/5);
        
        %Motor3
        if (M3Degree < p1(1,2))
            if(M3FirstStart == 0)
                start(M3);
                M3FirstStart = M3FirstStart+1;
            end
            M3.Speed = 0.3;%speeds(3)
        elseif (M3Degree > p1(1,2))
            if(M3FirstStart == 0)
                start(M3);
                M3FirstStart = M3FirstStart+1;
            end
            M3.Speed = -0.3;%speeds(3)
        else
            stop(M3);
        end
        M3P = readVoltage(a, 'A10');
        M3Degree = 360*(M3P/5);  
        
        
end

stop(M1);
stop(M2);
stop(M3);

%pause(10);
start(M1);
start(M2);
start(M3);

%Get back to default position
while ((M1Degree < 165 || M1Degree > 185) || (M2Degree < 190 || M2Degree > 210) || (M3Degree < 50 || M3Degree > 70))
    %Motor1
    if (M1Degree < 165 )
        M1.Speed = -0.2;
    elseif(M1Degree > 185)
        M1.Speed = 0.2;
    else
        stop(M1);
    end
    M1P = readVoltage(a, 'A8');
    M1Degree = 360*(M1P/5);

    %Motor2
    if (M2Degree < 190)
        M2.Speed = -0.2;
    elseif M2Degree > 210
        M2.Speed = 0.2;
    else
        stop(M2);
    end
    M2P = readVoltage(a, 'A9');
    M2Degree = 360*(M2P/5);
    %Motor3

    if (M3Degree < 50)
        M3.Speed = 0.25;
    elseif M3Degree > 70
        M3.Speed = -0.25;
    else
        stop(M3);
    end
    M3P = readVoltage(a, 'A10');
    M3Degree = 360*(M3P/5);
end
stop(M1);
stop(M2);
stop(M3);
