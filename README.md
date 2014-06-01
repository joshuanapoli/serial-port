serial-port
===========

This library implements the iostreams interface for serial ports (RS232, etc.).
It is a fork of Terraneo Federico's [serial-port][1] project, with a tutorial
[blog][2].


Example
-------

```c++
#include <iostream>
#include <serialstream.h>

using namespace std;
using namespace std::chrono;

int main(int argc, char* argv[])
{
  SerialOptions options;
  options.setDevice("/dev/ttyUSB0");
  options.setBaudrate(115200);
  options.setTimeout(seconds(3));
  options.setFlowControl(SerialOptions::software);
  options.setParity(SerialOptions::even);
  options.setCsize(7);
  SerialStream serial(options);
  serial.exceptions(ios::badbit | ios::failbit);
  serial<<"Hello world"<<endl;
  try
  {
    string s;
    getline(serial, s);
    cout << s << endl;
  }
  catch(TimeoutException&)
  {
    serial.clear(); //Don't forget to clear error flags after a timeout
    cerr << "Timeout occurred"<<endl;
  }
  return 0;
}
```


Building with Microsoft Visual Studio
-------------------------------------

The supplied MSBuild files will look for an installation of boost in the parent
directory of this repo. 

The boost directories and other build parameters can be
configured by creating a
[Directories.targets](msvc/Directories.targets.example) file.


[1]: https://gitorious.org/serial-port                            "Original serial-port repo"
[2]: http://www.webalice.it/fede.tft/serial_port/serial_port.html "Serial Ports and C++"