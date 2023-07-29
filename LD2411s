#include "esphome.h"

using namespace esphome;

class UARTSensor : public Component, public UARTDevice {
  public:
  UARTSensor(UARTComponent *parent) : UARTDevice(parent) {}
  Sensor* presence_sensor = new Sensor();
  Sensor* motion_sensor = new Sensor();
  Sensor* distance_sensor = new Sensor();
  Sensor* max_motion_sensor = new Sensor();
  Sensor* min_motion_sensor = new Sensor();
  Sensor* max_presence_sensor = new Sensor();
  Sensor* min_presence_sensor = new Sensor();
  Sensor* unocc_time_sensor = new Sensor();

  void setup() {
  }

  std::vector<int> bytes;

void loop() override
{
    while (available())
    {
        bytes.push_back(read());

        //End of Frame is 0x55 0x55
        if( (bytes[bytes.size()-2] == 0x55 && bytes[bytes.size()-1] == 0x55) || ( bytes[bytes.size()-4] == 0x04 && bytes[bytes.size()-3] == 0x03 && bytes[bytes.size()-2] == 0x02 && bytes[bytes.size()-1] == 0x01 ) )
        {            
						processPacket();
            bytes.clear();
        }
    }
}

  void processPacket() {            

    std::string str = "";
	
	if ((bytes[0] == 0xAA) && (bytes[1] == 0xAA) && (bytes[2] == 0x00)) { 
		
		presence_sensor->publish_state(0);
		motion_sensor->publish_state(0);
		
		return;
	}

  if ( (bytes[0] == 0xAA) && (bytes[1] == 0xAA) && (bytes[2] == 0x01) ) {        
      
		presence_sensor->publish_state(1);
		motion_sensor->publish_state(1);

    unsigned char byte3 = bytes[3];
    unsigned char byte4 = bytes[4];
    
		unsigned int distanceHex = (byte4 << 8) | byte3;
    
		int distanceCm = static_cast<int>(distanceHex);
		
		distance_sensor->publish_state(distanceCm);
		return;
	}

  if ( (bytes[0] == 0xAA) && (bytes[1] == 0xAA) && (bytes[2] == 0x02) ) {       
      
		presence_sensor->publish_state(1);
		motion_sensor->publish_state(0);

    unsigned char byte3 = bytes[3];
    unsigned char byte4 = bytes[4];
    
		unsigned int distanceHex = (byte4 << 8) | byte3;
    
		int distanceCm = static_cast<int>(distanceHex);
		
		distance_sensor->publish_state(distanceCm);

		return;
	}

  if ( (bytes[0] == 0xFD) && (bytes[1] == 0xFC) && (bytes[2] == 0xFB) && (bytes[3] == 0xFA) && (bytes[6] == 0x73) && (bytes[7] == 0x01) ) {
		
    unsigned char maxmlow = bytes[10];
    unsigned char maxmhigh = bytes[11];

    unsigned int maxmhex = (maxmhigh << 8) | maxmlow;

    int max_m = static_cast<int>(maxmhex);

    unsigned char minmlow = bytes[12];
    unsigned char minmhigh = bytes[13];

    unsigned int minmhex = (minmhigh << 8) | minmlow;

    int min_m = static_cast<int>(minmhex);

    unsigned char maxplow = bytes[14];
    unsigned char maxphigh = bytes[15];

    unsigned int maxphex = (maxphigh << 8) | maxplow;

    int max_p = static_cast<int>(maxphex);

    unsigned char minplow = bytes[16];
    unsigned char minphigh = bytes[17];

    unsigned int minphex = (minphigh << 8) | minplow;

    int min_p = static_cast<int>(minphex);

    unsigned char unocclow = bytes[18];
    unsigned char unocchigh = bytes[19];

    unsigned int unocchex = (unocchigh << 8) | unocclow;

    int unocc_time = static_cast<int>(unocchex);

    max_motion_sensor->publish_state(max_m);
		min_motion_sensor->publish_state(min_m);
		max_presence_sensor->publish_state(max_p);
		min_presence_sensor->publish_state(min_p);
		unocc_time_sensor->publish_state(unocc_time / 10);

		return;
	}

  }

};
