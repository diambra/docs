---
date: 2016-04-09T16:50:16+02:00
title: Human Experience Recorder
weight: 50
---

This example focuses on:

- Human experience recording settings configuration
- Controller interfacing (Gamepad or Keyboard)

{{% notice tip %}}
A dedicated section describing human experience recording wrapper settings is presented <a href="/imitationlearning/#experience-recording-wrapper">here</a> and provides additional details on their usage and purpose.
{{% /notice %}}

{{% notice note %}}
Depending on the Operating System used, specific permissions may be needed in order to read the keyboard inputs.<br><br> - On Windows, by default no specific permissions are needed. However, if you have some third-party security software you may need to white-list Python.<br> - On Linux you need to add the user the `input` group: `sudo usermod -aG input $USER`<br> - On Mac, it is possible you need to use the settings application to allow your program to access the input devices (see <a href="https://inputs.readthedocs.io/en/latest/user/install.html#mac-permissions" target="_blank">this reference</a>).<br><br>Official `inputs` python package reference guide can be found at <a href="https://inputs.readthedocs.io/en/latest/user/install.html#windows-permissions" target="_blank">this link</a>
{{% /notice %}}

{{< github_code "https://raw.githubusercontent.com/diambra/arena/main/examples/human_trajectory_recording_options_single_player.py" >}}