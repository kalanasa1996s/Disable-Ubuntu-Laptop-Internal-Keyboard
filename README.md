### How to Disable Ubuntu Laptop Internal Keyboard

If you want to disable your Ubuntu laptop’s internal keyboard, especially when using an external one, you can achieve this using `libinput` and `udev` rules. Here's a step-by-step guide to help you through the process:

#### Step 1: Install `libinput-tools`
First, ensure that `libinput-tools` is installed on your system:
```bash
sudo apt install libinput-tools
```

#### Step 2: Identify Your Internal Keyboard
Use the following command to list all input devices and locate your laptop’s internal keyboard:
```bash
libinput list-devices
```
Look for a section corresponding to your keyboard, like:
```
Device:           AT Translated Set 2 keyboard
Kernel:           /dev/input/event4
Capabilities:     keyboard
```
Note the event number associated with the keyboard, in this case, `/dev/input/event4`.

#### Step 3: Get Device Attributes
To get more detailed information about the device, run the following command, replacing `event4` with the correct event number from the previous step:
```bash
udevadm info -a -p /sys/class/input/event4
```
Look for a unique attribute, like:
```
ATTRS{name}=="AT Translated Set 2 keyboard"
```

#### Step 4: Create a Udev Rule to Disable the Keyboard
Now, create a Udev rule to ignore the internal keyboard.

1. Open a new file for the rule:
   ```bash
   sudo nano /etc/udev/rules.d/99-disable_keyboard.rules
   ```

2. Add the following rule:
   ```bash
   KERNEL=="event*", ATTRS{name}=="AT Translated Set 2 keyboard", ENV{LIBINPUT_IGNORE_DEVICE}="1"
   ```

3. Save and close the file.

#### Step 5: Test the Udev Rule
Before rebooting, test the rule to ensure it's being applied:
```bash
udevadm test /sys/class/input/event4
```
Check for the line `LIBINPUT_IGNORE_DEVICE=1` in the output to confirm it works.

#### Step 6: Reboot to Apply the Changes
Finally, reboot your system to apply the changes:
```bash
sudo reboot
```
After the reboot, your internal keyboard should be disabled.

### Conclusion
By following these steps, you can successfully disable your Ubuntu laptop’s internal keyboard. If you need to re-enable it in the future, simply delete the Udev rule file you created (`/etc/udev/rules.d/99-disable_keyboard.rules`) and reboot your system.
