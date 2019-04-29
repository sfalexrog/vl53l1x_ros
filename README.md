# vl53l1x_ros

This is STM [VL53L1X](https://www.st.com/en/imaging-and-photonics-solutions/vl53l1x.html) time-of-flight rangefinder driver for ROS. Tested on a Raspberry Pi 3 with [CJMCU-531](https://ru.aliexpress.com/item/VL53L1X/32911692450.html) board.

The code is based on the [STM VL53L1X API library](https://www.st.com/content/st_com/en/products/embedded-software/proximity-sensors-software/stsw-img007.html).

## Installation

### From package

For Raspberry Pi, there exist prebuilt Debian packages. For installation, [get the package](http://coex.space/rpi-ros-kinetic/pool/main/r/ros-kinetic-vl53l1x/) to the Raspberry and install it with `dpkg -i <package-name>.deb`.

### Manual

1. Clone the repository into your Catkin workspace:

    ```bash
    cd ~/catkin_ws/src
    git clone https://github.com/okalachev/vl53l1x_ros.git
    ```

2. Build the package:

    ```bash
    cd ~/catkin_ws
    catkin_make -DCATKIN_WHITELIST_PACKAGES="vl53l1x"
    ```

## Parameters

All parameters are optional.

<!-- * `~rate` (*double*) – measurement rate, *Hz* (default: 20). -->

* `~i2c_bus` (*int*) – I2C bus number (default: 1).
* `~i2c_address` (*int*) – I2C address (default: 0x29).
* `~mode` (*int*) – distance mode, 1 = short, 2 = medium, 3 = long (default: 3).
* `~timing_budget` (*double*) – timing budget for measurements, *s* (default: 0.1)
* `~poll_rate` (*double*) – polling data rate, *Hz* (default: 100).
* `~offset` (*float*) – offset to be automatically added to measurement value, *m* (default: 0.0).
* `~frame_id` (*string*) – frame id for output `Range` messages (deafult: "").
* `~field_of_view` (*float*) – field of view for output `Range` messages, *rad* (default: 0.471239).
* `~min_range` (*float*) – minimum range for output `Range` messages, *m* (default: 0.0).
* `~max_range` (*float*) – maximum range for `Range` output messages, *m* (default: 4.0).

`timing_budget` is the time VL53L1X uses for ranging. The larger this time, the more accurate is measument and the larger is maximum distance. Timing budget can be set from *0.02 s* up to *1 s*.

* *0.02 s* is the minimum timing budget and can be used only in *Short* distance mode.
* *0.033 s* is the minimum timing budget which can work for all distance modes.
* *0.14 s* is the timing budget which allows the maximum distance of *4 m* (in *Long* distance mode).

The resulting measurement rate is *1 / (timing budget + 0.004) Hz*.

`mode` is one of three distance modes, with the timing budget of *0.1 s*, *Short*, *Medimum* and *Long* modes have maximum distances of 136, 290, and 360 cm, respectively.

See the [official documentation](https://www.st.com/resource/en/datasheet/vl53l1x.pdf) for more information on mode and timing budget.

## Topics

### Published

* `~range` ([*sensor_msgs/Range*](http://docs.ros.org/kinetic/api/sensor_msgs/html/msg/Range.html)) – resulting measurement.

## Examples

Run with the default settings:

```bash
rosrun vl53l1x vl53l1x_node
```

Run the example [.launch-file](vl53l1x/launch/example.launch):

```bash
roslaunch vl53l1x example.launch
```

See the ranging results:

```bash
rostopic echo /vl53l1x/range
```

See the ranging rate:

```bash
rostopic hz /vl53l1x/range
```

## Copyright

Copyright © 2019 Oleg Kalachev.

Distributed under MIT License (https://opensource.org/licenses/MIT).
