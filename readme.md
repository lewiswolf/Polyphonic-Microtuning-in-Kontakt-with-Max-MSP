# Polyphonic Microtuning in Kontakt with Max MSP

## by Lewis Wolf

Written in Kontakt v5.5 & Max MSP v.7+  

The files contained in this repository demonstrate how to dynamically microtune Kontakt using Max MSP. Kontakt is unique in the way that it handles pitch bend information, allowing for each semitone to be divided into 100000 equal parts. The method shown here takes full advantage of this added layer of precision, and allows for real time flexibility without the need to alter the Kontakt script any further.

This method takes an incoming MIDI message in Max MSP and appends it with a tuning value (some number between -50000 and 50000, or rather a quarter tone in either direction). The tuning value is then transmitted to Kontakt using three control change messages. Once in Kontakt, the Kontakt script combines the three control change values to determine the pitch deviation. This is done using a `poly~`, which makes use of all 16 MIDI channels, so as to allow for polyphonic microtuning.

The Kontakt script was adapted from Olivier Pasquet's original microtuning script which can be found [here](https://www.opasquet.fr/microtuna/).

A tutorial video for this project can be found [here](https://youtu.be/bQRwxHz54rs).

## KSP Specifics

```
on init

	declare $lewtune
	declare polyphonic $DryTone_ID

end on

on note

	$DryTone_ID := $EVENT_ID
	change_tune ($DryTone_ID, $lewtune, 0)

end on

on controller

	if ($CC_NUM = 119)
	$lewtune := %CC[119]*1000+%CC[118]*10+%CC[117]-50000
	end if

end on
```

-   To use this script, paste it onto the first script that isn't a GUI related script.

-   If the script you are working in already contains `on init`, `on note` and `on controller` tags, do not rewrite them. Instead, paste the appropriate code within them.

-   Lastly, check all the scripts in the instrument for any other appearances of the `change_tune` function. If the `change_tune` function is used elsewhere, it doesn't need to be deleted. Simply change the third argument, the relevant bit, to 1 (see page 69 of the KSP Reference Manual).
