
Othermod:
The OS is watching the pin attached to GP1, so pressing the button issues the shutdown command. After shutdown completes, GP2 is pulled low by the OS. It's attached to a p-mosfet because the pin is only weakly pulled low. This causes the 10uf capacitor to charge, pulling the gate n-mosfet part of the dual mosfet low. This causes everything to shut off once the gate pin pulls low enough.

Georgeloak:
So the 300k pull down on the gate of the GP2 P-FET isn't enough to turn that FET ON and only when the system takes GP2 low it's enough to turn that P-FET on?

As a side note, I've been experimenting with overlays and it seems you can control this circuit without needing a python program. Adding

    dtoverlay=gpio-poweroff,gpiopin=21,active_low="y"
    dtoverlay=gpio-shutdown,gpio_pin=20

to /boot/config.txt seems to be working. Adjusting the value of the 10uF cap was needed in my case because the power off pin was taking a little long to go low after the halt was issued.

Othermod:
Correct. The 300k pulldown can't overcome the 40k pullup from the GPIO pin. As soon as the GPIO switches to pulldown or nopull, the 300k pulls the gate low and causes the poweroff.
