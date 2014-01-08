gps-com
=======
Final code => Temperature - Humidity - Pressure - Sending SMS
variables declaration 
int integer_value = 0;
String of char declaration char temperature_string[10];
Sprintf ==> Converts integer to string of chars sprintf(temperature_string,"Temp: %d%c%s",integer_value,' ',”hello”);
Joining two strings of chars
Utils.strCp(str1,str2);
// float2String ==> Converts float to string Utils.float2String(value_pressure,pressure_string,2);
Data structure
Num of mote <space> Temp. <espace> Hum. <espace> Pres. <espace> Battery
3 23.45 33.45 88.99 74
number to sent ==> 0704287385
/* Workshop on Applications of Wireless Sensor Networks
*Weather Monitoring in East Africa
*6 July - 8 July 2011
*
* Complete sensor example
*
*/
// var
float value_temperature = 15.33; float value_humidity = 20.44; float value_pressure = 55.66; int battery_level = 0;
char temperature_string[8]; char humidity_string[8]; char pressure_string[8];
char data_structure[200]; int mote_num = 1;
void setup(){ // USB begin USB.begin();
//Start GPRS GPRS.ON();
USB.println("GPRS module ready...");
//waiting while GPRS connects to the network while(!GPRS.check());
USB.println("GPRS connected to the network");
//configure SMS and Incoming Calls
if(GPRS.setInfoIncomingCall()) USB.println("Info Incoming Call OK"); if(GPRS.setInfoIncomingSMS()) USB.println("Info Incoming SMS OK"); if(GPRS.setTextModeSMS()) USB.println("Text Mode SMS OK");
// Sensors configure SensorAgr.setBoardMode(SENS_ON);
SensorAgr.setSensorMode(SENS_ON, SENS_AGR_TEMPERATURE); SensorAgr.setSensorMode(SENS_ON, SENS_AGR_HUMIDITY); SensorAgr.setSensorMode(SENS_ON, SENS_AGR_PRESSURE); delay(2000);
}
void loop()
{
// Read the sensors
value_temperature = SensorAgr.readValue(SENS_AGR_TEMPERATURE); value_humidity = SensorAgr.readValue(SENS_AGR_HUMIDITY); value_pressure = SensorAgr.readValue(SENS_AGR_PRESSURE);
battery_level = PWR.getBatteryLevel();
Utils.float2String(value_temperature,temperature_string,2);
Utils.float2String(value_humidity,humidity_string,2);
Utils.float2String(value_pressure,pressure_string,2);
// Building the data structure sprintf(data_structure,"%d%c%s%c%s%c%s%c%d",mote_num,' ',temperature_string,'
',humidity_string,' ',pressure_string,' ',battery_level);
// Sending SMS
if (GPRS.sendSMS(data_structure,"0704287385")) USB.println("SMS sent"); else USB.println("Error sending SMS");
delay(1200000);
}

