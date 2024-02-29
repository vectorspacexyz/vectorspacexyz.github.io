---
title: switch audio sink
---

```bash
#!/bin/bash

#Script to move every input source to another sink using pactl command line and dmenu

# pick an audio output from the list
list_sinks() {
    DEFAULT="$(pactl info | grep 'Default Sink:' | cut -d' ' -f3)"
    # pactl list sinks short  | cut -d '        ' -f2 | grep -v "$DEFAULT" | head --lines 1
    pactl list sinks short | grep -v 'Generic_[^1]' | grep -v "$DEFAULT"  | cut -d '    ' -f2 | dmenu -i
}


# move every audio source and change the default sink
change_audio_sink(){

    sink_inputs=$(pactl list sink-inputs | grep 'Sink Input' | cut -d'#' -f2)

    # if [[ -z "$sink_inputs" ]]
    # then
    #     notify-send -t 1000 -h string:x-canonical-private-synchronous:volume-notification "Sink unchanged."
    #     exit
    # fi

    pactl set-default-sink "$1"
    while read -r input; do
        # move existing stream to inactive sink
        pactl move-sink-input "$input" "$1"
    done <<< "$sink_inputs"

    notify-send -t 1000 -h string:x-canonical-private-synchronous:volume-notification "Current sink: $1"
}

choice=$(list_sinks)
change_audio_sink "$choice"
```
