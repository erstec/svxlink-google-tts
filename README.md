# svxlink-google-tts
### Tools to create SvxLink sound files using Googles Cloud TTS


This is the README file for generating SvxLink sound clips using the Google
text to speech system (https://cloud.google.com/text-to-speech). 

Google Cloud Text-to-Speech enables developers to synthesize natural-sounding 
speech with 30 voices, available in multiple languages and variants. It creates
extremely high quality speech and can easily be used to add additional messages 
to SvxLink.

You have to have a Google Cloud account and an API key to use this code.  Start
by following the directions found here:

https://cloud.google.com/text-to-speech/docs/quickstart-protocol

Install the API key .json file in ~/.google/<your key filename.json>

Make sure you have python and pip installed.   
Then install the python virtualenv with 'pip install virtualenv'

To generate sound files start by checking out the code.

    git clone http://gitub.com/n7ipb/svxlink-google-tts
    cd svxlink-google-tts

The directory contains all the directories and text files to duplicate those found in 
svxlink-sounds-en_US-heather.  In addition it contains a new directory (Custom) in 
which you can place your own custom text files for specialty messages.   The default 
Custom directory contains a number of entries that I use and others may find useful.
Add your own as necessary.

There are seven support files/scripts.

    README - this file
    
    google_tts.cfg - the default configuration file that identifies which subdirecories
    to search for text files and the language, voice ,pitch, speed and other paramaters.  
    The default values create a pleasant sounding female voice and builds all the SvxLink 
    sound files.
    
    custom_google_tts.cfg - The same entries as google_tts.cfg with the exception of
    building only those found in the Custom directory.
    
    requirements.txt - Google TTS version number needed.
    
    list_voices.py - support program used by the main program to list available voices
    
    synthesize_text.py - Python program to set paramaters and call the API with the
    text to be converted.  This program is not used directly but is instead called by
    svxlink-google-tts.sh.

    svxlink-google-tts.sh - the main program that traverses the directories and creates 
    all the .wav files. 

    Usage: svxlink-google-tts.sh [-L] <config file> "
            -L -- List available Voices"

    Invoked with the -L option it simply prints a list of available voices and exits.
    Without any paramaters it defaults to using the google_tts.cfg file or if given 
    a filename it will attempt to interpret it as a custom config.
    
    Example: ./svxlink-google-tts.sh custom_google_tts.cfg
        This creates .wav files for all the entries found in the Custom directory.
        

# Example config file:

```

###############################################################################
#
# Default configuration file for svxlink-google-tts.sh 
#
# Modify this or create your own custom config file with a different name
#
###############################################################################
# Set which subdirectories to search through for text files to convert
# 
SUBDIRS="Custom,Core,DtmfRepeater,Parrot,SelCallEnc,Trx,Default,EchoLink,Frn,Help,MetarInfo,PropagationMonitor,TclVoiceMail"

#
#
# Customizations for the synthesis
#
# For full descriptions of the following see;
# https://cloud.google.com/text-to-speech/docs/reference/rpc/google.cloud.texttospeech.v1beta1#google.cloud.texttospeech.v1beta1.TextToSpeech.SynthesizeSpeech
# 
LANGUAGE="en_US"
# run the script with the -L option to obtain a list of available names
# ./svxlink-google-tts.sh -L
NAME="en-US-Wavenet-C"
# Gender is often overridden by the choices available for a given name
# so this may have no noticable effect
GENDER="FEMALE"
# Speaking rate - 0.25 to 4.0, 1.0 is normal rate
RATE="1.0"
# Speaking pitch - -20.0,20.0, each step is a semitone up or down
PITCH="-2.0"
# Samplerate for the audio synthesis.   Use 16000 for svxlink
SAMPLERATE="16000"
#
# Type of text files - Ascii text (text),  Synthesized Speech Markup Language (ssml)
# at the moment it's all files as text or all as ssml.   By default all the existing SvxLink text 
# files are not using ssml.
TEXTTYPE="text"

```

# To use:
    Make sure you have an API .json key installed in ~/.google
    
    cd to the repository.
    
    Edit svxlink-google-tts.sh and insert your API key filename where it says
    "PLACE_YOUR_API_KEY_FILENAME_HERE"
    
    To build everything:
    ./svxlink-google-tts.sh
    
    Test sound samples with 'aplay <path to wave file>'
    
    When you're satisfied with the results you can create a new language pack with
    
    create_language_pack.sh.  
    
    The result will be a .tgz file of the form:
   ``` 
   svxlink_sounds_<NAME>_<GENDER>_<RATE>_<PITCH>.tgz
   Note: The file includes a copy of the config file used to generate the sounds.
   ```
