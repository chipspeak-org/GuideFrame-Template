# GuideFrame-Template 2
This repository exists as a template to allow users to experiment with GuideFrame.

it includes:

* A GuideFrame script.
* Its accompanying markdown file.
* Instructions on how to modify it for your own purposes.
* workflow for rendering the aforementioned GuideFrame script.

## Instructions

### Local Installation
GuideFrame is designed to be used within a CI/CD pipeline. However, if you wish to use it locally you can simply fork this repo, clone it locally and run the following:
```bash
  sudo apt-get update
  sudo apt-get install -y \
    ffmpeg \
    xvfb \
    chromium-driver \
    chromium-browser
    pip install guideframe
```
NOTE: The above is an ubuntu example (which presumes installation of pip and python).

Once it has been installed locally, simply run `python <guideframe_script_name> <operating_system_arg>`
NOTE: GuideFrame currently only supports macos via the arg: `macos` and ubuntu via the arg: `github`. More os arguments are intended in the future.


### GitHub Installation
To use GuideFrame in a repo of your choice, simply copy the workflow and scripts within this repo as starting points. All you need is a 
GuideFrame script, corresponding markdown voiceover script and a workflow with which to trigger them.


### Demo Script
The `guideframe_demo.py` script serves as a template to get you started. It uses the tutors.dev website as an example but the same
functions can be used to interact with hrefs and elements from any website. 

Each `guide_step` function takes a step number (corresponding to the step in the accompanying markdown), function(s) passed to lambda and
an 'order' argument. To illustrate this further, lets use an example from the demo.

```python
  '''
  Step 5 - Hovering over the first card
  '''
  guide_step(
      5,
      lambda: hover_over_element(driver, "/note/tutors-reference-manual/unit-0-getting-started/note-01-getting-started"),
      order="action-before-vo"
      )
```

Within the above example, '5' is passed as the step argument. This always corresponds to the step within the matching markdown file. This text is in turn passed
to the audio logic to create the voiceover and match it to the appropriate video step.

`lambda` allows you to pass a function from the selenium-sdk. A wide range of functions is available within this sdk and is detailed in the GuideFrame docs
and can also be viewed here: https://github.com/chipspeak/GuideFrame/blob/main/guideframe/selenium.py

Finally, each `guide_step` features a default argument for `order`. The default is "action-after-vo" but in the above example we've used the other option to place our voiceover after the interaction. This allows you to experiment with the video's pacing as you see fit.

### Demo Workflow
The `render.yaml` workflow is triggered on push events and works to run the GuideFrame script on a GitHub runner before uploading the result as an artefact. The logic of the
workflow should be apparent. It once again serves as a simple starting point but it is advised to avoid manipulating the `Run Main Script With Display started` job in order to avoid compromising the render.
