# Tab Through Google Search Results with Safari

## Overview

This AppleScript scans and sends keystrokes to an open Safari browser window to allow you to easily tab between the previous and next links on the search results web page returned by Google's search engine.

While this script can work alone from the command line, it's best used with a hot key application to execute it. For example, you can trigger the script with a simple Automator service (or "Quick Action," as Apple refers to them now). For the impatient, a third party hot key application like Karabiner-Elements will be more responsive to your keystrokes than Automator. Since I use Karabiner-Elements, I have provided the JSON file you can use to tab up/down with the `CTRL-J` (next link) and `CTRL-K` (previous link) keys. The JSON file can be downloaded from this repository.

## Motivation

For whatever very bad reason (likely related to squeezing every drop of ad revenue from their product), Google does not allow you to easily tab through its search results. When logged in to Google, you have to hit tab 17 times just to get to the first result. And it can take anywhere from a few taps or dozens more to get to the results further down the page. This makes tabbing impractical for navigating the page.

There may be a Safari web extension that already does this but I couldn't find it easily, so I wrote this for fun and to brush up on my barely existent AppleScript skills.

## Installation

Below are the basic steps for installing and using the script and assumes you have a decent understanding of scripting and how to set them up with Automator or your favorite hot key app. More detailed instructions for less experienced users will follow in the future as time permits. In the meantime, feel free to ask a question in the issue queue if you need help.

0. In Safari, go to Preferences => Advanced and make sure "Press Tab to highlight each item on a webpage" is enabled.
1. Download the `tab_to_result.osa` AppleScript from this repository
2. Add the AppleScript to a directory in your shell's $PATH
3. Set permissions to make the script executable
4. Follow the instructions below in the "Testing the script" section below
5. Configure your third party app to configure your chosen hot keys to trigger the script

### Testing the script

Before configuring your hotkey application, you should test the script to make sure it works by opening a Google search results page in Safari. Then, from a terminal window, navigate to the directory containing the script and run:

`./tab_to_result.osa`

Safari should activate and then tab move quickly to the first search result on the page. Execute the script once each time you want to move to the next tab.

To test moving up to the previous link, run:

`./tab_to_result.osa up`

Now the previous search result should be highlighted and the screen will move down to center the highlighted search result on the page.

### Setting up your hot key app

When configuring your hot key app, the "next" hot key, which tabs to the next Google search result on the page, should execute the AppleScript with no arguments. The "previous" hot key should pass a single "up" argument the script.

#### Automator "Quick Action"

Here is some basic guidance for getting Automator "Quick Actions" set up to work with the AppleScript:

[Official "Quick Action" documentation](https://support.apple.com/guide/automator/use-quick-action-workflows-aut73234890a/mac)

It's a good idea to set the "Quick Action" works with just Safari to prevent other applications to hot keys meant for Safari.

You will need to create two actions: one that executes the "previous" hot key for moving up the page to the next result and one that triggers the "next" hot key for going down.

Once your "Quick Actions" are set up, you need to connect them to a keyboard shortcut. Here's a helpful [StackOverfow post you may find useful](https://apple.stackexchange.com/questions/175215/how-do-i-assign-a-keyboard-shortcut-to-an-applescript-i-wrote). If you have a touch bar, you can add the shortcut there as well. Consult Google for further details.

**CAUTION** If the hot key combination is the control key followed by another single key, CTRL-D for example, you will likely run into a problem with the script causing the Safari to cycle through open tabs instead of links on the page. See the **Problems?** section below for options to work around this.

#### Karabiner-Elements

Here are the basic steps to getting this working with Karabiner-Elements:

1. Download the JSON file provided in the repository
2. Place the file in the proper configuration directory for Karabiner-Elements
3. Install the hot keys into Karabiner elements

Consult the official [Karabiner-Elements website](https://karabiner-elements.pqrs.org).

## How It Works

The AppleScript has comments to help you see how it works in more detail. But basically, it just mindlessly hits the tab key and uses a dash of JavaScript to look at the `InnerHTML` property of the current active element to see if it's on something that looks like a search result. If it is, then it stops hitting the tab key.

There are most certainly going to be better ways to do this than repeatedly hitting tab but that would entail more coding than I'm willing to put the time and effort into. This works good enough for me. I'm not a seasoned JavaScript/AppleScript developer so feel free to submit a patch to make the script a little more sensible.

## Known Issues

### Links get skipped

See the **Problems?** section below for a fix.

## Problems?

### With Automator, the script tabs through the open tabs if I keep the `control` key pressed

This happens when you set the script to be activated by pressing the `control` key along with a single other key.

The easiest fix is to change the hot key so it uses a different modifier key like `command` or uses multiple modifier keys, command-control. Another option is to change the default hot keys in Safari so control-tab and control-shift-tab does not cycle through your open tabs. Or you can just remember to release the control key after each activation, but this is not ideal.

### The script is skipping over links

This happens because the script is not waiting long enough for the next tab to be highlighted. The simple fix is to edit the AppleScript and on or about line 23 change the delay to a higher delay. If it's set too low, results may be skipped. If you set it too high, it will take longer to get to the next result. Adjust the delay until you hit on the right setting through trial and error.

The current setting is .003 seconds, which is the setting I use for my high performance iMac. There's a good chance you will need to adjust this setting upward for best results.

### I can't get application X to trigger the hot key!

Sorry, I am unable help you figure out the particulars of your hot key application but I'm willing to post up information to this README if you get it figured out.

### I installed the script but nothing happens when I run it from the command line

First, make sure you are on a Google search result page. Then see step 0 of the installation steps. If it still fails, open up and issue and report any errors you might be getting.
