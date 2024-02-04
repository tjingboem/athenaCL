# Tutorial 4: Creating and Editing Textures

This tutorial demonstrates basic Texture creation, configuration, and deployment
in musical structures. This chapter is essential for using athenaCL for
algorithmic music production.



## Introduction to Textures and ParameterObjects

A TextureInstance (or a Texture or TI) is an algorithmic music layer. Like a
track or a part, a Texture represents a single musical line somewhat analogous
to the role of a single instrument in an ensemble. The music of a Texture need
not be a single monophonic line: it may consist of chords and melody, multiple
independent lines, or any combination or mixture. The general generative shape
and potential of a Texture is defined by the TextureModule. A Texture is an
instance of a TextureModule: a single TextureModule type can be used to create
many independent instances of that type; each of these instances can be
customized and edited independently. Collections of TextureInstances are used to
create an EventList, or the musical output of all Textures.
      
A TextureInstance consists of many configurable slots, or attributes. These
attributes allow the user to customize each Texture. Attributes include such
properties as timbre (instrument and parametric timbre specifications), rhythm
(duration and tempo), frequency materials (Path, transposition, and octave
position), and mixing (amplitude and panning). Other attributes may control
particular features of the Texture, like the number of voices, position of
chords, or formal properties.
      
Most attributes of a TextureInstance are not fixed values. Unlike a track or a
part, a Texture often does not have a fixed sequence of values for attributes
like amplitude, or even fixed note-sequences. Rather, attributes of a Texture
are algorithmic objects, or ParameterObjects. Rather than enter a value for
amplitude, the user chooses a ParameterObject to produce values for the desired
attribute, and enters settings to specialize the ParameterObject's
behavior. Rather than enter note-sequences, the Texture selects and combines
pitches from a Path, or a user-supplied sequence of pitch groups. In this way
each attribute of a Texture can be given a range of motion and a degree of
indeterminacy within user-composed boundaries.
      
A TextureInstance is not a fixed entity: it is a collection of instructions on
how to create events for a certain duration. Every time an EventList is created,
each Texture is "performed," or called into motion to produce events. Depending
on the TextureModule and the Texture's configuration, the events produced may be
different each time the EventList is created.
      
athenaCL is designed to allow users work with broad outlines of musical
parameters and materials, and from this higher level organize and control
combinations of Textures. This should not be confused with a much higher level
of algorithmic composition, where an algorithm is responsible for creating an
entire composition: its style, form, parts, and surface. athenaCL is unlikely to
produce such "complete" musical structures. Rather, athenaCL is designed to
produce complex, detailed, and diverse musical structures and
surfaces. Combinations of parts and construction of form are left to the user,
and can be composed either in athenaCL or in a Digital Audio Workstation where
athenaCL EventOutput formats, such as MIDI files or Csound-rendered audio files,
can be mixed, processed, and combined in whatever desired
fashion. Alternatively, MIDI files produced with athenaCL can be modified or
combined in traditional sequencers and notation editors.
      



## Introduction Instrument Models

athenaCL features numerous integrated instrument models. In some cases these
instrument models are references to external specifications; in other cases
these instrument models contain complete source code necessary for instantiating
synthesis systems. Textures are assigned an instrument from an Orchestra upon
creation, and are able to control a wide variety of instrument-specific
parameters.
      
athenaCL features an integrated library of Csound instruments, providing
automated control of both Csound score and orchestra generation and control. For
details on installing and using Csound within athenaCL, see . Csound instruments
are signal processing and synthesis instructions. These instructions designates
a certain number of parameters to expose to the user of the instrument. These
parameters allow events in the score to communicate information and settings to
the instrument. athenaCL's integrated library of Csound instruments permits
dynamically constructed orchestra files to be used with athenaCL-generated
Csound scores. Alternatively, users can use external, custom orchestras with
athenaCL-written score files. EventModes csoundNative, csoundExternal, and
csoundSilence support diverse ways of working with Csound within athenaCL.
      
athenaCL provides instrument collections (Orchestras) for working with other
EventOutput formats. For working with MIDI systems, General MIDI (GM) instrument
definitions are provided with the generalMidi and generalMidiPercussion
EventModes.
      
Whenever a Texture is created, an instrument must be specified by number. This
is necessary because the Texture must be configured with additional
ParameterObjects for instrument-specific parameter control. Instruments are
always identified by a number, though within athenaCL descriptive names are
provided when available.
      
The instruments available during Texture creation are dependent on the active
EventMode: that is, for any active EventMode, one Orchestra is available from
which a Texture's instrument must be selected. In the following example, the
user lists available EventModes to check that csoundNative is active, and then
views the available instruments with the EMi command.

**Listing available Instruments with EMi**

```
pi{}ti{} :: emls
EventMode modes available:
{name}
   csoundExternal      
 + csoundNative        
   csoundSilence       
   midi                
   midiPercussion      
   superColliderNative 

pi{}ti{} :: emi
csoundNative instruments:
{number,name}
   3      sineDrone                        
   4      sineUnitEnvelope                 
   5      sawDrone                         
   6      sawUnitEnvelope                  
   11     noiseWhite                       
   12     noisePitched                     
   13     noiseUnitEnvelope                
   14     noiseTambourine                  
   15     noiseUnitEnvelopeBandpass        
   16     noiseSahNoiseUnitEnvelope        
   17     noiseSahNoiseUnitEnvelopeDistort 
   20     fmBasic                          
   21     fmClarinet                       
   22     fmWoodDrum                       
   23     fmString                         
   30     samplerReverb                    
   31     samplerRaw                       
   32     samplerUnitEnvelope              
   33     samplerUnitEnvelopeBandpass      
   34     samplerUnitEnvelopeDistort       
   35     samplerUnitEnvelopeParametric    
   36     samplerSahNoiseUnitEnvelope      
   40     vocodeNoiseSingle                
   41     vocodeNoiseSingleGlissando       
   42     vocodeNoiseQuadRemap             
   43     vocodeNoiseQuadScale             
   44     vocodeNoiseQuadScaleRemap        
   45     vocodeNoiseOctScale              
   46     vocodeNoiseOctScaleRemap         
   47     vocodeNoiseBiOctScale            
   48     vocodeNoiseTriOctScale           
   50     guitarNylonNormal                
   51     guitarNylonLegato                
   52     guitarNylonHarmonic              
   60     additiveBellBright               
   61     additiveBellDark                 
   62     additiveBellClear                
   70     synthRezzy                       
   71     synthWaveformVibrato             
   72     synthVcoAudioEnvelopeSineQuad    
   73     synthVcoAudioEnvelopeSquareQuad  
   74     synthVcoDistort                  
   80     pluckTamHats                     
   81     pluckFormant                     
   82     pluckUnitEnvelope                
   110    noiseAudioEnvelopeSineQuad       
   111    noiseAudioEnvelopeSquareQuad     
   130    samplerAudioEnvelopeSineQuad     
   131    samplerAudioEnvelopeSquareQuad   
   132    samplerAudioFileEnvelopeFilter   
   133    samplerAudioFileEnvelopeFollow   
   140    vocodeSineOctScale               
   141    vocodeSineOctScaleRemap          
   142    vocodeSineBiOctScale             
   143    vocodeSineTriOctScale            
   144    vocodeSineQuadOctScale           
   145    vocodeSinePentOctScale           
   146    vocodeSineHexOctScale            
   230    samplerVarispeed                 
   231    samplerVarispeedAudioSine        
   232    samplerVarispeedReverb           
   233    samplerVarispeedDistort          
   234    samplerVarispeedSahNoiseDistort  
   240    vocodeVcoOctScale                
   241    vocodeVcoOctScaleRemap    
```

Other EventModes provide other Orchestras for use in Textures. In the example
below, the user selects the EventMode midiPercussion with the EMo command and
examines the available instruments with the EMi command:
      

**Examining additional Instruments with EMi**

```
pi{}ti{} :: emo mp
EventMode mode set to: midiPercussion.

pi{}ti{} :: emi
generalMidiPercussion instruments:
{number,name}
   35     acousticBassDrum 
   36     bassDrum1        
   37     sideStick        
   38     acousticSnare    
   39     handClap         
   40     electricSnare    
   41     lowFloorTom      
   42     closedHiHat      
   43     highFloorTom     
   44     pedalHiHat       
   45     lowTom           
   46     openHiHat        
   47     lowMidTom        
   48     hiMidTom         
   49     crashCymbal1     
   50     highTom          
   51     rideCymbal1      
   52     chineseCymbal    
   53     rideBell         
   54     tambourine       
   55     splashCymbal     
   56     cowBell          
   57     crashCymbal2     
   58     vibraSlap        
   59     rideCymbal2      
   60     hiBongo          
   61     lowBongo         
   62     muteHiConga      
   63     openHiConga      
   64     lowConga         
   65     highTimbale      
   66     lowTimbale       
   67     highAgogo        
   68     lowAgogo         
   69     cabasa           
   70     maracas          
   71     shortWhistle     
   72     longWhistle      
   73     shortGuiro       
   74     longGuiro        
   75     claves           
   76     hiWoodBlock      
   77     lowWoodBlock     
   78     muteCuica        
   79     openCuica        
   80     muteTriangle     
   81     openTriangle    
```




## Selecting and Viewing TextureModules

A Texture is an instance of a TextureModule. Every time a Texture is created,
athenaCL creates an independent instance of the active TextureModule. To display
a complete list of all available TextureModules, enter the command TMls:
      

**Listing TextureModules with TMls**

```
pi{}ti{} :: tmls
TextureModules available:
{name,TIreferences}
   DroneArticulate    0
   DroneSustain       0
   HarmonicAssembly   0
   HarmonicShuffle    0
   InterpolateFill    0
   InterpolateLine    0
   IntervalExpansion  0
   LineCluster        0
 + LineGroove         0
   LiteralHorizontal  0
   LiteralVertical    0
   MonophonicOrnament 0
   TimeFill           0
   TimeSegment        0
```

As in other athenaCL displays, the first line of the display is a key, telling
the user that the list consists of a name followed by the number of TI
references. This number displays the count of TextureInstances referenced from a
parent TextureModule. The "+" designates the active TextureModule. When creating
a new TextureInstance, athenaCL uses the active TextureModule.
      
To select a different TextureModule, the user enters TMo. The user is prompted
to enter the name or number (as represented in the list order) of the desired
TextureModule. The TMls command can be used to confirm the change.
      

**Selecting the active TextureModule with TMo**

```
pi{}ti{} :: tmo 
which TextureModule to activate? (name or number 1-14): da
TextureModule DroneArticulate now active.

pi{}ti{} :: tmls
TextureModules available:
{name,TIreferences}
 + DroneArticulate    0
   DroneSustain       0
   HarmonicAssembly   0
   HarmonicShuffle    0
   InterpolateFill    0
   InterpolateLine    0
   IntervalExpansion  0
   LineCluster        0
   LineGroove         0
   LiteralHorizontal  0
   LiteralVertical    0
   MonophonicOrnament 0
   TimeFill           0
   TimeSegment        0
```

Here the user has entered "da", to select the TextureModule
DroneArticulate. Whenever selecting objects in athenaCL the user may enter the
acronym (formed from the leading character and all following capitals), the
literal name ("dronearticulate"), or the ordinal number as displayed in the
corresponding list display.
      
To learn what a particular TextureModule does, as well what types of Texture
options are available, enter the command TMv, for TextureModule View. In this
example, the user, with TIo, selects the TextureModule "LineGroove" (with a
command-line argument) before entering the TMv command.
      

**Viewing details of the active TextureModule**

```
pi{}ti{} :: tmo linegroove
TextureModule LineGroove now active.

pi{}ti{} :: tmv
TextureModule: LineGroove; author: athenaCL native
This TextureModule performs each set of a Path as a simple monophonic line;
pitches are chosen from sets in the Path based on the pitch selector control.
texture (s)tatic
parallelMotionList    Description: List is a collection of transpositions
                      created above every Texture-generated base note. The
                      timeDelay value determines the amount of time in seconds
                      between each successive transposition in the
                      transpositionList. Arguments: (1) name, (2)
                      transpositionList, (3) timeDelay
pitchSelectorControl  Description: Define the selector method of Path pitch
                      selection used by a Texture. Arguments: (1) name, (2)
                      selectionString {'randomChoice', 'randomWalk',
                      'randomPermutate', 'orderedCyclic',
                      'orderedCyclicRetrograde', 'orderedOscillate'}
levelFieldMonophonic  Description: Toggle between selection of local field
                      (transposition) values per set of the Texture Path, or per
                      event. Arguments: (1) name, (2) level {'set', 'event'}
levelOctaveMonophonic Description: Toggle between selection of local octave
                      (transposition) values per set of the Texture Path, or per
                      event. Arguments: (1) name, (2) level {'set', 'event'}
texture (d)ynamic
```

The TMv command displays the name of the TextureModule along with the author of
its code. Following the author designation is a description of how the module
performs. Following this is documentation for all TextureStatic parameter
objects, or Texture-specific options and user-configurable settings pertinent to
the particular TextureModule's algorithmic design.
      



## Creating, Selecting, and Viewing TextureInstances

A TextureInstance is always linked to a Path. If no Paths exists when the
Texture is created, a default Path is automatically created consisting of a
single Multiset with a single pitch (middle C, or C4). If Paths exists when the
Texture is created, the active PathInstance is assigned to the Texture. A
TextureInstance's Path can be later edited. For a complete introduction to Paths
see .
      
A new TextureInstance is always created from the active TextureModule; the user
must then always select the desired TextureModule before creating a Texture of
the desired type. A TextureInstance's type, or TextureModule, cannot be changed
after the Texture is created.
      
A new Texture is created with the TIn command, for TextureInstance New. The user
is prompted to name the new Texture and select an instrument by number. If the
number of the desired instrument is not known, a "?" can be entered to display a
list of instruments. In the example below the user selects TextureMode
LineGroove, EventMode midiPercussion, and then creates a texture named "a1" with
instrument 64 ("lowConga").
      

**Creating a new TextureInstance with TIn**

```
pi{}ti{} :: tmo linegroove
TextureModule LineGroove now active.

pi{}ti{} :: emo mp
EventMode mode set to: midiPercussion.

pi{}ti{} :: tin
name this texture: a1
enter instrument number:
(35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,6
1,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81)
or "?" for instrument help: 64
TI a1 created.
```

To hear the resulting musical structure, enter the command ELn. (For more
information on using ELn, see . The resulting MIDI file may be opened with the
ELh command.
      

**Creating a new EventList with ELn**

```
pi{auto-lowConga}ti{a1} :: eln
command.py: temporary file: /Volumes/xdisc/_scratch/ath2010.07.02.17.51.42.xml 
      EventList ath2010.07.02.17.51.42 complete:
/Volumes/xdisc/_scratch/ath2010.07.02.17.51.42.mid
/Volumes/xdisc/_scratch/ath2010.07.02.17.51.42.xml
```

After creating a Texture, the TIv command can be used to view the active
Texture:
      

**Viewing a TextureInstance**

```
pi{auto-lowConga}ti{a1} :: tiv
TI: a1, TM: LineGroove, TC: 0, TT: TwelveEqual
pitchMode: pitchSpace, silenceMode: off, postMapMode: on
midiProgram: piano1
      status: +, duration: 000.0--20.06
(i)nstrument        64 (generalMidiPercussion: lowConga)
(t)ime range        00.0--20.0
(b)pm               constant, 120
(r)hythm            pulseTriple, (constant, 4), (basketGen, randomPermutate,
                    (1,1,2,3)), (constant, 1), (constant, 0.75)
(p)ath              auto-lowConga
                    (E4)
                    20.00(s)
local (f)ield       constant, 0
local (o)ctave      constant, 0
(a)mplitude         randomBeta, 0.4, 0.4, (constant, 0.7), (constant, 0.9)
pan(n)ing           constant, 0.5
au(x)iliary         none
texture (s)tatic
      s0            parallelMotionList, (), 0.0
      s1            pitchSelectorControl, randomPermutate
      s2            levelFieldMonophonic, event
      s3            levelOctaveMonophonic, event
texture (d)ynamic   none
```

The TIv command displays all essential attributes of a Texture. Each label in
the display corresponds to an attribute in the TextureInstance. The TIv display
is in two-blocks. The first block gives parameters that are constant. The first
line displays the name of the TextureInstance (a1), the name of the parent
TextureModule (LineGroove), the number of TextureClones (0), and the active
TextureTemperament (TwelveEqual). The second line displays the PitchMode
(pitchSpace), the silenceMode (off), and the postMapMode (on). The third line
provides the GM MIDI program name (piano1). The fourth, indented line displays
the TextureInstance's mute status (where a "o" is muted and a "+" is non-muted)
and the absolute duration the Texture's events.
      
The second block lists the primary algorithmic controls of the Texture. The
names of these attributes use parenthesis to designate a single-letter
abbreviation. The instrument attribute is displayed first, with the value
following the label: instrument number (64), the name of the orchestra
(generalMidiPercussion) and the name of the instrument (lowConga). The next
attribute is time-range, the start and end time in seconds from the beginning of
the EventList. A new Texture is given a default time-range of twenty seconds
(00.0--20.0). New Textures, when created, get their time-range from the active
Texture.
      
The bpm attribute is the tempo in beats per minute. The value is set with the
ParameterObject "constant" to produce a tempo of 120 BPM. In most cases, the bpm
control is used to calculate the duration of rhythms and pulses used in a
Texture.
      
The rhythm attribute designates a Rhythm ParameterObject to control the
generation of event durations. Rhythm ParameterObjects often notate rhythms as
lists of Pulses. A Pulse is designated as a list of three elements: (divisor,
multiplier, accent). The duration of a rhythm is calculated by dividing the time
of a beat (from the bpm parameter) by the Pulse divisor, then multiplying the
result by the Pulse multiplier. The value of the "accent" determines if a
duration is a note or a rest, where 1 or a "+" designates a note, 0 or a "o"
designates a rest. Thus an eighth note is given as (2,1,1), a dotted-quarter
note as (2,3,1), and dotted-eighth rest as (4,3,0). In the example above, the
ParameterObject "loop" is used with three Pulses: two sixteenth notes (4,1,1)
and a duration equal to a quarter-note tied to a sixteenth note (4,5,1).
      
The Path attribute gives the name of the PathInstance used by this Texture,
followed on the next line by the Multiset pitches that will be
used. PathInstances are linked to the Texture. Thus, if a change is made to a
Path (with PIe, for example), all Textures that use that Path will reflect the
change. Each TextureInstance, however, can control the interpretation of a Path
in numerous ways. The Texture PitchMode setting, for example, determines if
pitches are derived from a Path in pitchSpace, pitchClassSpace, or as a
setClass. The local field and local octave attributes permit each Texture to
transpose pitches from the Path independently.
      
The attribute "local field" stores a ParameterObject that controls local
transposition of Path pitches. Values are given in semitones, and can include
microtonal transpositions as floating-point values following the semitone
integer. Thus, a transposition of five half-steps and a quarter-tone would be
equal to 5.5. A transposition of a major tenth would be 16. In the example above
the attribute value instructs the Texture to use a ParameterObject called
"constant." Note: some EventOutput formats do not support microtonal pitch
specification. In such cases microtones are rounded to the nearest semitone. The
attribute "local octave," similar to local field, controls the octave position
of Path pitches. Each integer represents an octave shift, where 0 is no octave
shift, each Path pitch retaining its original octave register.
      
The amplitude attribute designates a ParameterObject to control the amplitude of
the Texture, measured in a symbolic range from 0 to 1. The panning attribute
designates the ParameterObject used to control spatial location in stereo or
quadraphonic space. Values are along the unit interval, from 0 to 1.
      
The attributes that make up the "auxiliary" listing provide any number of
additional ParameterObjects to control instrument specific parameter fields. The
number of parameter fields is determined by the instrument definition.
      
The last attributes, "texture static" and "texture dynamic," designate controls
specific to particular TextureModules. The values here can be edited like other
attributes.
      
A second Texture will be created with TIn named "b1" and using
instrument 62. The Texture, after creation, is displayed with the TIv command.
      

**Creating and viewing a TextureInstance**

```
pi{auto-lowConga}ti{a1} :: tin
name this texture: b1
enter instrument number:
(35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,6
1,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81)
or "?" for instrument help: 62
TI b1 created.

pi{auto-muteHiConga}ti{b1} :: tiv
TI: b1, TM: LineGroove, TC: 0, TT: TwelveEqual
pitchMode: pitchSpace, silenceMode: off, postMapMode: on
midiProgram: piano1
      status: +, duration: 000.0--20.06
(i)nstrument        62 (generalMidiPercussion: muteHiConga)
(t)ime range        00.0--20.0
(b)pm               constant, 120
(r)hythm            pulseTriple, (constant, 4), (basketGen, randomPermutate,
                    (1,1,2,3)), (constant, 1), (constant, 0.75)
(p)ath              auto-muteHiConga
                    (D4)
                    20.00(s)
local (f)ield       constant, 0
local (o)ctave      constant, 0
(a)mplitude         randomBeta, 0.4, 0.4, (constant, 0.7), (constant, 0.9)
pan(n)ing           constant, 0.5
au(x)iliary         none
texture (s)tatic
      s0            parallelMotionList, (), 0.0
      s1            pitchSelectorControl, randomPermutate
      s2            levelFieldMonophonic, event
      s3            levelOctaveMonophonic, event
texture (d)ynamic   none
```

This new Texture, though created with the same TextureModule, is a completely
autonomous object. No changes to "a1" will have any effect on "b1".
      
During an athenaCL session a user can create any number of TextureInstances and
save this collection in an AthenaObject file for latter use. For more
information on saving, loading, and merging AthenaObjects see . To view a list
of all current Textures, enter the command TIls, for TextureInstance List.
      

**Listing all TextureInstances**

```
pi{auto-muteHiConga}ti{b1} :: tils
TextureInstances available:
{name,status,TM,PI,instrument,time,TC}
   a1               + LineGroove  auto-lowConga    64  00.0--20.0   0
 + b1               + LineGroove  auto-muteHiConga 62  00.0--20.0   0
```

This display shows a list of all Textures, each Texture on a single line. The
information given, in order from left to right, is the name, the mute-status,
the parent TM, the PathInstance, the instrument number, the time-range, and the
number of TextureClones. Notice the "+" in front of Texture "b1": this
designates that this Texture is active. To change the active Texture, enter the
command TIo either with a command-line argument or alone:
      

**Selecting the active TextureInstance**

```
pi{auto-muteHiConga}ti{b1} :: tio a1
TI a1 now active.

pi{auto-muteHiConga}ti{a1} :: 
```

In order to compare a single attribute of all Textures, the user can enter the
command TEv, for TextureEnsemble View. TextureEnsemble refers to the collection
of all Textures, and all TE commands process all Textures simultaneously. The
user will be prompted to enter an abbreviation for the desired
attribute. Attribute abbreviations are notated in the TIv display labels. Thus
the attribute abbreviation for "(a)mplitude" is "a"; the attribute abbreviation
for "pan(n)ing" is "n." As with other commands, use of command-line arguments
provides flexible control:
      

**Viewing parameter values for all Textures**

```
pi{auto-muteHiConga}ti{a1} :: tev
compare texture parameters: which parameter? a
compare parameters: amplitude
{name,value,}
a1                  randomBeta, 0.4, 0.4, (constant, 0.7), (constant, 0.9)  
b1                  randomBeta, 0.4, 0.4, (constant, 0.7), (constant, 0.9) 

pi{auto-muteHiConga}ti{a1} :: tev i
compare parameters: instrument
{name,value,}
a1                  64 (generalMidiPercussion: lowConga)     
b1                  62 (generalMidiPercussion: muteHiConga)  
```




## Copying and Removing Texture Instances

TextureInstances can be duplicated with the command TIcp. The user is prompted
to enter the name of the Texture to be copied, and then the name of the
copy. The copy can be confirmed by listing all Textures with the command TIls.

**Copying a TextureInstance**

```
pi{auto-muteHiConga}ti{a1} :: ticp
which TextureInstnace to copy? (name or number 1-2): b1
name this copy of TI 'b1': b2
TextureInstance b2 created.

pi{auto-muteHiConga}ti{b2} :: tils
TextureInstances available:
{name,status,TM,PI,instrument,time,TC}
   a1               + LineGroove  auto-lowConga    64  00.0--20.0   0
   b1               + LineGroove  auto-muteHiConga 62  00.0--20.0   0
 + b2               + LineGroove  auto-muteHiConga 62  00.0--20.0   0
```

Textures can be deleted with the command TIrm, for TextureInstance Remove. The
user is prompted to enter the name of the Texture to be deleted. The removal can
be confirmed by listing all Textures with the command TIls.
      

**Removing a TextureInstance**

```
pi{auto-muteHiConga}ti{b2} :: tirm
which TextureInstnace to delete? (name or number 1-3): b2
are you sure you want to delete texture b2? (y, n, or cancel): y
TI b2 destroyed.

pi{auto-muteHiConga}ti{b1} :: tils
TextureInstances available:
{name,status,TM,PI,instrument,time,TC}
   a1               + LineGroove  auto-lowConga    64  00.0--20.0   0
 + b1               + LineGroove  auto-muteHiConga 62  00.0--20.0   0   

pi{auto-muteHiConga}ti{b1} :: 
```

When the active Texture is deleted, as it is above, athenaCL chooses a new
Texture to activate, here choosing "b1." To select a different Texture, use the
command TIo.
      



## Editing TextureInstance Attributes

Each attribute of a Texture can be edited to specialize its performance. Some
attributes such as instrument, time-range, and Path are static: they do not
change over the duration of a Texture. Other attributes are dynamic, such as
bpm, rhythm, local field, local octave, amplitude and panning, and can be
configured with a wide range of ParameterObjects.
      
Texture attributes are edited with the TIe command. The command first prompts
the user to select which attribute to edit. Attributes are named by a
single-letter abbreviation, as notated in the TIv display with
parenthesis. Next, the current value of the attribute is displayed, followed by
a prompt for the new value. In the following example the time range of Texture
"a1" is edited:
      

**Editing a TextureInstance**

```
pi{auto-muteHiConga}ti{b1} :: tie
edit TI b1
which parameter? (i,t,b,r,p,f,o,a,n,x,s,d): t
current time range: 0.0, 20.0
new value: 5, 20
TI b1: parameter time range updated.

pi{auto-muteHiConga}ti{b1} :: tiv
TI: b1, TM: LineGroove, TC: 0, TT: TwelveEqual
pitchMode: pitchSpace, silenceMode: off, postMapMode: on
midiProgram: piano1
      status: +, duration: 005.0--19.97
(i)nstrument        62 (generalMidiPercussion: muteHiConga)
(t)ime range        05.0--20.0
(b)pm               constant, 120
(r)hythm            pulseTriple, (constant, 4), (basketGen, randomPermutate,
                    (1,1,2,3)), (constant, 1), (constant, 0.75)
(p)ath              auto-muteHiConga
                    (D4)
                    15.00(s)
local (f)ield       constant, 0
local (o)ctave      constant, 0
(a)mplitude         randomBeta, 0.4, 0.4, (constant, 0.7), (constant, 0.9)
pan(n)ing           constant, 0.5
au(x)iliary         none
texture (s)tatic
      s0            parallelMotionList, (), 0.0
      s1            pitchSelectorControl, randomPermutate
      s2            levelFieldMonophonic, event
      s3            levelOctaveMonophonic, event
texture (d)ynamic   none
```

In the example above the user select "t" to edit the active Texture's time-range
attribute. In general, new values for attributes must be entered with the same
syntax with which they are displayed. In this example, time-range values are
given as two numbers separated by a comma. Deviation from this syntax will
return an error. The user enters 5, 20 to set the time-range attribute to the
duration from 5 to 20 seconds.
      
The command TEe, for TextureEnsemble Edit can be used to edit the entire
collection of Textures with one command. In the following example the user
selects the amplitude attribute with "a" and then enters a new ParameterObject:
randomUniform. The randomUniform parameterObject produces random values with a
uniform distribution between the required arguments for minimum and
maximum. After this edit, TEv, with the command-line argument "a", can be used
to view the amplitude for all Textures and confirm the edit.
      

**Editing a single parameter of all Textures with TEe**

```

pi{auto-muteHiConga}ti{b1} :: tee
edit all TextureInstances
which parameter? (i,t,b,r,p,f,o,a,n,x): a
sample amplitude: randomBeta, 0.4, 0.4, (constant, 0.7), (constant, 0.9)
new value: ru, .6, 1
TI a1: parameter amplitude updated.
TI b1: parameter amplitude updated.

pi{auto-muteHiConga}ti{b1} :: tev a
compare parameters: amplitude
{name,value,}
a1                  randomUniform, (constant, 0.6), (constant, 1)  
b1                  randomUniform, (constant, 0.6), (constant, 1) ```
```

Using ELn, the current collection of Textures can be used to create an
EventList, and ELh may be used to audition the results. (For more information on
using ELn, see .) The random fluctuation of amplitude values should provide a
variety of accent patterns to the fixed rhythmic loop.
      
The collection of Textures can be displayed in a graphical and textual diagram
produced by the TEmap command. This command lists each Texture and Clone within
the current AthenaObject and provides a proportional representation of their
respective start and end times.
      

**Generating a graphical display of Texture position with TEmap**

```
pi{auto-muteHiConga}ti{b1} :: temap
TextureEnsemble Map:
19.97s              |      .       |      .       |      .      |       .      |
a1                  ____________________________________________________________
b1                                 _____________________________________________
```




## Muting Textures

Textures can be muted to disable the inclusion of their events in all
EventOutputs. Textures and their Clones (see ) can be muted independently. The
command TImute, if no arguments are given, toggles the current Texture's mute
status. The following example demonstrates muting Texture a1, listing all
Textures with with TIls, and then displaying the collection of Textures with the
TEmap command. Notice that in the TIls display, the "status" of Texture a1 is
set to "o", meaning that it is muted.
      

**Muting a Texture with TImute**

```

pi{auto-muteHiConga}ti{b1} :: timute
TI b1 is now muted.

pi{auto-muteHiConga}ti{b1} :: tils
TextureInstances available:
{name,status,TM,PI,instrument,time,TC}
   a1               + LineGroove  auto-lowConga    64  00.0--20.0   0
 + b1               o LineGroove  auto-muteHiConga 62  05.0--20.0   0

pi{auto-muteHiConga}ti{b1} :: temap
TextureEnsemble Map:
19.97s              |      .       |      .       |      .      |       .      |
a1                  ____________________________________________________________
b1                                 _____________________________________________
```

By providing the name of one or more Textures as command-line arguments,
numerous Texture's mute status can be toggled. In the following example, Texture
a1 is given as an argument to the TImute command. The TIls command shows that
the Texture is no longer muted.
      

**Removing mute status with TImute**

```
pi{auto-muteHiConga}ti{b1} :: timute a1
TI a1 is now muted.

pi{auto-muteHiConga}ti{b1} :: tils
TextureInstances available:
{name,status,TM,PI,instrument,time,TC}
   a1               o LineGroove  auto-lowConga    64  00.0--20.0   0
 + b1               o LineGroove  auto-muteHiConga 62  05.0--20.0   0

pi{auto-muteHiConga}ti{b1} :: timute a1
TI a1 is no longer muted.

pi{auto-muteHiConga}ti{b1} :: timute b1
TI b1 is no longer muted.
```




## Viewing and Searching ParameterObjects

For each dynamic attribute of a TextureInstance, a ParameterObject can be
assigned to produce values over the duration of the Texture. Complete
documentation for all ParameterObjects can be found in . Texture attributes for
bpm, local field, local octave, amplitude, panning, and all auxiliary parameters
(if required by the instrument) can have independent ParameterObjects.
      
ParameterObjects are applied to a Texture attribute with an argument
list. athenaCL accepts lists in the same comma-separated format of Python list
data structures. A list can consist of elements like strings, numbers, and other
lists, each separated by a comma. Within athenaCL, text strings need not be in
quotes, and sub-lists can be given with either parenthesis or brackets. Each
entry in the ParameterObject argument list corresponds, by ordered-position, to
an internal setting within the ParameterObject. The first entry in the argument
list is always the name of the ParameterObject. ParameterObject names, as well
as all ParameterObject configuration strings, can always be accessed with
acronyms.
      
To display a list if all available ParameterObjects, enter the command TPls, for
TextureParameter list:
      

**Displaying all ParameterObjects with TPls**

```
pi{auto-muteHiConga}ti{b1} :: tpls
Generator ParameterObject
{name}
   accumulator                
   basketFill                 
   basketFillSelect           
   basketGen                  
   basketSelect               
   breakGraphFlat             
   breakGraphHalfCosine       
   breakGraphLinear           
   breakGraphPower            
   breakPointFlat             
   breakPointHalfCosine       
   breakPointLinear           
   breakPointPower            
   caList                     
   caValue                    
   constant                   
   constantFile               
   cyclicGen                  
   directorySelect            
   envelopeGeneratorAdsr      
   envelopeGeneratorTrapezoid 
   envelopeGeneratorUnit      
   feedbackModelLibrary       
   fibonacciSeries            
   funnelBinary               
   grammarTerminus            
   henonBasket                
   iterateCross               
   iterateGroup               
   iterateHold                
   iterateSelect              
   iterateWindow              
   lineSegment                
   listPrime                  
   logisticMap                
   lorenzBasket               
   markovGeneratorAnalysis    
   markovValue                
   mask                       
   maskReject                 
   maskScale                  
   noise                      
   oneOver                    
   operatorAdd                
   operatorCongruence         
   operatorDivide             
   operatorMultiply           
   operatorPower              
   operatorSubtract           
   pathRead                   
   quantize                   
   randomBeta                 
   randomBilateralExponential 
   randomCauchy               
   randomExponential          
   randomGauss                
   randomInverseExponential   
   randomInverseLinear        
   randomInverseTriangular    
   randomLinear               
   randomTriangular           
   randomUniform              
   randomWeibull              
   sampleAndHold              
   sampleSelect               
   sieveFunnel                
   sieveList                  
   staticInst                 
   staticRange                
   typeFormat                 
   valuePrime                 
   valueSieve                 
   waveCosine                 
   waveHalfPeriodCosine       
   waveHalfPeriodPulse        
   waveHalfPeriodSine         
   waveHalfPeriodTriangle     
   wavePowerDown              
   wavePowerUp                
   wavePulse                  
   waveSawDown                
   waveSawUp                  
   waveSine                   
   waveTriangle               

Rhythm Generator ParameterObject
{name}
   binaryAccent         
   convertSecond        
   convertSecondTriple  
   gaRhythm             
   iterateRhythmGroup   
   iterateRhythmHold    
   iterateRhythmWindow  
   loop                 
   markovPulse          
   markovRhythmAnalysis 
   pulseSieve           
   pulseTriple          
   rhythmSieve          

Filter ParameterObject
{name}
   bypass               
   filterAdd            
   filterDivide         
   filterDivideAnchor   
   filterFunnelBinary   
   filterMultiply       
   filterMultiplyAnchor 
   filterPower          
   filterQuantize       
   maskFilter           
   maskScaleFilter      
   orderBackward        
   orderRotate          
   pipeLine             
   replace              
```

To display detailed documentation for a ParameterObject, enter the command TPv,
for Texture Parameter view. In the following example the user views the
ParameterObjects "wavePowerDown" and "noise" by providing command-line arguments
for the desired ParameterObject name:
      

**Viewing ParameterObject reference information**

```
pi{auto-muteHiConga}ti{b1} :: tpv wpd
Generator ParameterObject
{name,documentation}
WavePowerDown       wavePowerDown, stepString, parameterObject, phase, exponent,
                    min, max
                    Description: Provides a power down wave between 0 and 1 at a
                    rate given in either time or events per period. Depending on
                    the stepString argument, the period rate (frequency) may be
                    specified in spc (seconds per cycle) or eps (events per
                    cycle). This value is scaled within the range designated by
                    min and max; min and max may be specified with
                    ParameterObjects. The phase argument is specified as a value
                    between 0 and 1. Note: conventional cycles per second (cps
                    or Hz) are not used for frequency. Arguments: (1) name, (2)
                    stepString {'event', 'time'}, (3) parameterObject
                    {secPerCycle}, (4) phase, (5) exponent, (6) min, (7) max

pi{auto-muteHiConga}ti{b1} :: tpv noise
Generator ParameterObject
{name,documentation}
Noise               noise, resolution, parameterObject, min, max
                    Description: Fractional noise (1/fn) Generator, capable of
                    producing states and transitions between 1/f white, pink,
                    brown, and black noise. Resolution is an integer that
                    describes how many generators are used. The gamma argument
                    determines what type of noise is created. All gamma values
                    are treated as negative. A gamma of 0 is white noise; a
                    gamma of 1 is pink noise; a gamma of 2 is brown noise; and
                    anything greater is black noise. Gamma can be controlled by
                    a dynamic ParameterObject. The value produced by the noise
                    generator is scaled within the unit interval. This
                    normalized value is then scaled within the range designated
                    by min and max; min and max may be specified by
                    ParameterObjects. Arguments: (1) name, (2) resolution, (3)
                    parameterObject {gamma value as string or number}, (4) min,
                    (5) max
```

The command TPmap provides graphical displays of ParameterObject-generated
values. (To configure athenaCL graphics output, see .) The user must supply the
name of the ParamaterObject library (Generator, Rhythm, or Filter), the number
of events to generate, and the ParameterObject argument list.
      

**ParameterObject Map display with TPmap**

```
pi{auto-muteHiConga}ti{b1} :: tpmap
select a library: Generator, Rhythm, or Filter. (g, r, f): g
number of events: 120
enter a Generator ParameterObject argument: wpd, e, 30, 0, 2

wavePowerDown, event, (constant, 30), 0, 2, (constant, 0), (constant, 1)
TPmap display complete.
```

The TPmap, like other athenaCL commands, can be used with command-line
arguments. In the following example, the user produces a TPmap display of the
noise ParameterObject, generating "brown" fractional noise:
      

**ParameterObject Map display with TPmap**

```
pi{auto-muteHiConga}ti{b1} :: tpmap 120 n,50,(c,2),0,1

noise, 50, (constant, 2), (constant, 0), (constant, 1)
TPmap display complete.
```




## Editing ParameterObjects

To edit an attribute of Texture, a user enters a new ParameterObject argument
list. The command TIe, as before, first prompts the user to select which
attribute to edit. Next, the current value of the attribute is displayed,
followed by a prompt for the new value. TIv can be used to confirm the changed
value. In the following example, the panning of Texture "a1" is assigned a
fractional noise (1/f) ParameterObject:
      

**Editing the panning of a TextureInstance**

```
pi{auto-muteHiConga}ti{b1} :: tie
edit TI b1
which parameter? (i,t,b,r,p,f,o,a,n,x,s,d): n
current panning: constant, 0.5
new value: n, 50, (cg, ud, 1, 3, .2), .5, 1
TI b1: parameter panning updated.

pi{auto-muteHiConga}ti{b1} :: tiv
TI: b1, TM: LineGroove, TC: 0, TT: TwelveEqual
pitchMode: pitchSpace, silenceMode: off, postMapMode: on
midiProgram: piano1
      status: o, duration: 005.0--19.97
(i)nstrument        62 (generalMidiPercussion: muteHiConga)
(t)ime range        05.0--20.0
(b)pm               constant, 120
(r)hythm            pulseTriple, (constant, 4), (basketGen, randomPermutate,
                    (1,1,2,3)), (constant, 1), (constant, 0.75)
(p)ath              auto-muteHiConga
                    (D4)
                    15.00(s)
local (f)ield       constant, 0
local (o)ctave      constant, 0
(a)mplitude         randomUniform, (constant, 0.6), (constant, 1)
pan(n)ing           noise, 50, (cyclicGen, upDown, 1, 3, 0.2), (constant, 0.5),
                    (constant, 1)
au(x)iliary         none
texture (s)tatic
      s0            parallelMotionList, (), 0.0
      s1            pitchSelectorControl, randomPermutate
      s2            levelFieldMonophonic, event
      s3            levelOctaveMonophonic, event
texture (d)ynamic   none
```

The noise ParameterObject has been given an embedded ParameterObject to control
the gamma argument. Notice that instead of entering "noise", "cyclicGen" or
"upDown" the user can enter the acronyms "n", "cg", and "ud". All
ParameterObjects support automatic acronym expansion of argument strings. This
is an important and time-saving shortcut.
      
The previous example edited the panning of Texture "a1" such that it produces
values within the range of .5 to 1. This limits the spatial location of the
sound to the upper half of the range (the middle to right stereo position). To
limit the spatial location of "b1" in a complementary fashion, the Texture is
edited to produce values within the range 0 to .5, corresponding to the lower
half of the range (the middle to left stereo position). In the example below,
TIo is used to select "b1" before entering the TIe command. TEv is then used to
compare panning values for all Textures.
      

**Editing the panning of a TextureInstance**

```
pi{auto-muteHiConga}ti{b1} :: tio b1
TI b1 now active.

pi{auto-muteHiConga}ti{b1} :: tie n wpd,e,15,.25,2.5,0,.5
TI b1: parameter panning updated.

pi{auto-muteHiConga}ti{b1} :: tiv
TI: b1, TM: LineGroove, TC: 0, TT: TwelveEqual
pitchMode: pitchSpace, silenceMode: off, postMapMode: on
midiProgram: piano1
      status: o, duration: 005.0--20.06
(i)nstrument        62 (generalMidiPercussion: muteHiConga)
(t)ime range        05.0--20.0
(b)pm               constant, 120
(r)hythm            pulseTriple, (constant, 4), (basketGen, randomPermutate,
                    (1,1,2,3)), (constant, 1), (constant, 0.75)
(p)ath              auto-muteHiConga
                    (D4)
                    15.00(s)
local (f)ield       constant, 0
local (o)ctave      constant, 0
(a)mplitude         randomUniform, (constant, 0.6), (constant, 1)
pan(n)ing           wavePowerDown, event, (constant, 15), 0.25, 2.5, (constant,
                    0), (constant, 0.5)
au(x)iliary         none
texture (s)tatic
      s0            parallelMotionList, (), 0.0
      s1            pitchSelectorControl, randomPermutate
      s2            levelFieldMonophonic, event
      s3            levelOctaveMonophonic, event
texture (d)ynamic   none

pi{auto-muteHiConga}ti{b1} :: tev n
compare parameters: panning
{name,value,}
a1                  constant, 0.5
b1                  wavePowerDown, event, (constant, 15), 0.25, 2.5, (constant,
                    0), (constant, 0.5)
```

Notice that, in the above example, the user provided complete command-line
arguments for the TIe command. When entering a ParameterObject from the
command-line, no spaces, and only commas, can be used between ParameterObject
arguments. As command-line arguments are space delimited (and ParameterObject
arguments are comma delimited), a ParameterObject on the command line must be
given without spaces between arguments. When providing a ParameterObject to a
TIe prompt, however, spaces may be provided.
      



## Editing Rhythm ParameterObjects

Rhythm ParameterObjects are ParameterObjects specialized for generating time and
rhythm information. Many Rhythm ParameterObjects use Pulse object notations to
define proportional rhythms and reference a Texture's dynamic bpm
attribute. Other ParameterObjects are independent of bpm and can use raw timing
information provided by one or more Generator ParameterObjects.
      
When using proportional rhythms, athenaCL uses Pulse objects. Pulses represent a
ratio of duration in relation to the duration of beat (specified in BPM and
obtained from the Texture). For details on Pulse notation, enter "help pulse":
      

**View Pulse and Rhythm help**

```
pi{auto-muteHiConga}ti{b1} :: help pulse
{topic,documentation}
Pulse and Rhythm    Pulses represent a duration value derived from ratio and a
                    beat-duration. Beat duration is always obtained from a
                    Texture. Pulses are noted as a list of three values: a
                    divisor, a multiplier, and an accent. The divisor and
                    multiplier must be positive integers greater than zero.
                    Accent values must be between 0 and 1, where 0 is a measured
                    silence and 1 is a fully sounding event. Accent values my
                    alternatively be notated as + (for 1) and o (for 0). If a
                    beat of a given duration is equal to a quarter note, a Pulse
                    of (1,1,1) is a quarter note, equal in duration to a beat. A
                    Pulse of (2,1,0) is an eighth-note rest: the beat is divided
                    by two and then multiplied by one; the final zero designates
                    a rest. A Pulse of (4,3,1) is a dotted eight note: the beat
                    is divided by four (a sixteenth note) and then multiplied by
                    three; the final one designates a sounding event. A Rhythm
                    is designated as list of Pulses. For example: ((4,2,1),
                    (4,2,1), (4,3,1)).
```

To edit the rhythms used by Texture b1, enter TIe followed by an "r" to access
the rhythm attribute. As before, the user is presented with the current value,
then prompted for a new value. In the following example, the ParameterObject
"loop" is examined first with the TPv, then the active Texture is edited by
providing an random walk over an expanded rhythm. Finally, the rhythm attribute
of all Textures is viewed with TEv.
      

**Editing Rhythm ParameterObjects with TIe**

```
pi{auto-muteHiConga}ti{b1} :: tpv loop
Rhythm Generator ParameterObject
{name,documentation}
Loop                loop, pulseList, selectionString
                    Description: Deploys a fixed list of rhythms. Pulses are
                    chosen from this list using the selector specified by the
                    selectionString argument. Arguments: (1) name, (2) pulseList
                    {a list of Pulse notations}, (3) selectionString
                    {'randomChoice', 'randomWalk', 'randomPermutate',
                    'orderedCyclic', 'orderedCyclicRetrograde',
                    'orderedOscillate'}

pi{auto-muteHiConga}ti{b1} :: tie
command.py: raw args  
edit TI b1
which parameter? (i,t,b,r,p,f,o,a,n,x,s,d): r
current rhythm: pulseTriple, (constant, 4), (basketGen, randomPermutate,
(1,1,2,3)), (constant, 1), (constant, 0.75)
new value: l, ((4,1,1),(4,1,1),(4,2,1),(4,3,1),(4,5,1),(4,3,1)), rw
TI b1: parameter rhythm updated.

pi{auto-muteHiConga}ti{b1} :: tev r
compare parameters: rhythm
{name,value,}
a1                  pulseTriple, (constant, 4), (basketGen, randomPermutate,
                    (1,1,2,3)), (constant, 1), (constant, 0.75)
b1                  loop, ((4,1,+),(4,1,+),(4,2,+),(4,3,+),(4,5,+),(4,3,+)),
                    randomWalk
```

Notice that, as with all ParameterObjects, abbreviations can be used for
argument strings. The user need only enter the string "l" to select the "loop"
RhythmObject, and "rw" to select the randomWalk selection method.
      
To edit Texture a1, the user must first make a1 the active texture with TIo. In
the following example, the user applies a zero-order Markov chain to generate
pulses. The user fist consults the documentation for ParameterObject
markovPulse. For more information about Markov transition strings (Ariza 2006
[AN#1343]), enter "help markov". After selecting and editing the Texture, the
Rhythms are compared with TEv:
      

**Editing Rhythm ParameterObjects with TIe**

```
pi{auto-muteHiConga}ti{b1} :: tpv markovp
Rhythm Generator ParameterObject
{name,documentation}
markovPulse         markovPulse, transitionString, parameterObject
                    Description: Produces Pulse sequences by means of a Markov
                    transition string specification and a dynamic transition
                    order generator. The Markov transition string must define
                    symbols that specify valid Pulses. Markov transition order
                    is specified by a ParameterObject that produces values
                    between 0 and the maximum order available in the Markov
                    transition string. If generated-orders are greater than
                    those available, the largest available transition order will
                    be used. Floating-point order values are treated as
                    probabilistic weightings: for example, a transition of 1.5
                    offers equal probability of first or second order selection.
                    Arguments: (1) name, (2) transitionString, (3)
                    parameterObject {order value}

pi{auto-muteHiConga}ti{b1} :: tio a1
TI a1 now active.

pi{auto-muteHiConga}ti{a1} :: tie
edit TI a1
which parameter? (i,t,b,r,p,f,o,a,n,x,s,d): r
current rhythm: pulseTriple, (constant, 4), (basketGen, randomPermutate,
(1,1,2,3)), (constant, 1), (constant, 0.75)
new value: mp, a{8,1,1}b{4,3,1}c{4,2,1}d{4,5,1}:{a=1|b=3|c=4|d=7}, (c,0)
TI a1: parameter rhythm updated.

pi{auto-muteHiConga}ti{a1} :: tev r
compare parameters: rhythm
{name,value,}
a1                  markovPulse,
                    a{8,1,1}b{4,3,1}c{4,2,1}d{4,5,1}:{a=1|b=3|c=4|d=7},
                    (constant, 0)
b1                  loop, ((4,1,+),(4,1,+),(4,2,+),(4,3,+),(4,5,+),(4,3,+)),
                    randomWalk
                    
```

In the previous example, the user supplies four Pulses; each pulses is weighted
such that the shortest, (8,1,1), is the least frequent (weight of 1), and the
longest, (4,5,1), is the most frequent (weight of 7).
      
Using ELn, the current collection of Textures can be used to create an
EventList, and ELh may be used to audition the results. (For more information on
using ELn, see .) Each time an EventList is created, different sequences of
rhythms will be generated: for Texture a1, these rhythms will be the result of a
zero-order Markov chain; for Texture b1, these rhythms will be the result of a
random walk on an ordered list of Pulses.
      
A final alternation can be made to the metric performance of these
Textures. Using the TEe command, both Texture's bpm attribute can be altered to
cause a gradual accelerando from 120 BPM to 300 BPM. In the following example,
the user applies a wavePowerUp ParameterObject to the bpm attribute of both
Textures by using the TEe command with complete command-line arguments:
      

**Editing BPM with TEe**

```
pi{auto-muteHiConga}ti{a1} :: tee b wpu,t,20,0,2,120,300
TI a1: parameter bpm updated.
TI b1: parameter bpm updated.
```




## Editing Instruments and Altering EventMode

A Texture's instrument can be edited like other Texture attributes. The
instruments available for editing, just as when creating a Texture, are
dependent on the active EventMode. To use instruments from another EventMode,
the active EventMode must first be changed, and then the Texture may be assigned
an instrument.
      
In the following example, the user changes the EventMode to csoundNative with
EMo, examines the available instruments with EMi, and then assigns each Texture
instrument 80:
      

**Changing EventMode and editing Texture instrument**

```
pi{auto-muteHiConga}ti{a1} :: emo cn
EventMode mode set to: csoundNative.

pi{auto-muteHiConga}ti{a1} :: emi
csoundNative instruments:
{number,name}
   3      sineDrone                        
   4      sineUnitEnvelope                 
   5      sawDrone                         
   6      sawUnitEnvelope                  
   11     noiseWhite                       
   12     noisePitched                     
   13     noiseUnitEnvelope                
   14     noiseTambourine                  
   15     noiseUnitEnvelopeBandpass        
   16     noiseSahNoiseUnitEnvelope        
   17     noiseSahNoiseUnitEnvelopeDistort 
   20     fmBasic                          
   21     fmClarinet                       
   22     fmWoodDrum                       
   23     fmString                         
   30     samplerReverb                    
   31     samplerRaw                       
   32     samplerUnitEnvelope              
   33     samplerUnitEnvelopeBandpass      
   34     samplerUnitEnvelopeDistort       
   35     samplerUnitEnvelopeParametric    
   36     samplerSahNoiseUnitEnvelope      
   40     vocodeNoiseSingle                
   41     vocodeNoiseSingleGlissando       
   42     vocodeNoiseQuadRemap             
   43     vocodeNoiseQuadScale             
   44     vocodeNoiseQuadScaleRemap        
   45     vocodeNoiseOctScale              
   46     vocodeNoiseOctScaleRemap         
   47     vocodeNoiseBiOctScale            
   48     vocodeNoiseTriOctScale           
   50     guitarNylonNormal                
   51     guitarNylonLegato                
   52     guitarNylonHarmonic              
   60     additiveBellBright               
   61     additiveBellDark                 
   62     additiveBellClear                
   70     synthRezzy                       
   71     synthWaveformVibrato             
   72     synthVcoAudioEnvelopeSineQuad    
   73     synthVcoAudioEnvelopeSquareQuad  
   74     synthVcoDistort                  
   80     pluckTamHats                     
   81     pluckFormant                     
   82     pluckUnitEnvelope                
   110    noiseAudioEnvelopeSineQuad       
   111    noiseAudioEnvelopeSquareQuad     
   130    samplerAudioEnvelopeSineQuad     
   131    samplerAudioEnvelopeSquareQuad   
   132    samplerAudioFileEnvelopeFilter   
   133    samplerAudioFileEnvelopeFollow   
   140    vocodeSineOctScale               
   141    vocodeSineOctScaleRemap          
   142    vocodeSineBiOctScale             
   143    vocodeSineTriOctScale            
   144    vocodeSineQuadOctScale           
   145    vocodeSinePentOctScale           
   146    vocodeSineHexOctScale            
   230    samplerVarispeed                 
   231    samplerVarispeedAudioSine        
   232    samplerVarispeedReverb           
   233    samplerVarispeedDistort          
   234    samplerVarispeedSahNoiseDistort  
   240    vocodeVcoOctScale                
   241    vocodeVcoOctScaleRemap              

pi{auto-muteHiConga}ti{a1} :: tie i 80
baseTexture.py: WARNING: new Texture auxiliary value 2 
TI a1: parameter instrument updated.

pi{auto-muteHiConga}ti{a1} :: tio b1
TI b1 now active.

pi{auto-muteHiConga}ti{b1} :: tie i 80
baseTexture.py: WARNING: new Texture auxiliary value 2 
TI b1: parameter instrument updated.

pi{auto-muteHiConga}ti{b1} :: tiv
TI: b1, TM: LineGroove, TC: 0, TT: TwelveEqual
pitchMode: pitchSpace, silenceMode: off, postMapMode: on
midiProgram: piano1
      status: o, duration: 005.0--20.12
(i)nstrument        80 (csoundNative: pluckTamHats)
(t)ime range        05.0--20.0
(b)pm               wavePowerUp, time, (constant, 20), 0, 2, (constant, 120),
                    (constant, 300)
(r)hythm            loop, ((4,1,+),(4,1,+),(4,2,+),(4,3,+),(4,5,+),(4,3,+)),
                    randomWalk
(p)ath              auto-muteHiConga
                    (D4)
                    15.00(s)
local (f)ield       constant, 0
local (o)ctave      constant, 0
(a)mplitude         randomUniform, (constant, 0.6), (constant, 1)
pan(n)ing           wavePowerDown, event, (constant, 15), 0.25, 2.5, (constant,
                    0), (constant, 0.5)
au(x)iliary
      x0            cyclicGen, up, 0.1, 0.9, 0.1
      x1            cyclicGen, down, 800, 16000, 200
texture (s)tatic
      s0            parallelMotionList, (), 0.0
      s1            pitchSelectorControl, randomPermutate
      s2            levelFieldMonophonic, event
      s3            levelOctaveMonophonic, event
texture (d)ynamic   none
```

Notice that, after editing the Texture, a warning is issued. This warning tells
the user that additional auxiliary ParameterObjects have been added. As a
Csound-based instrument, each event of instrument 80 can accept two additional
synthesis parameters. When viewing a Texture with this instrument, as shown
above, the auxiliary display shows two additional ParameterObjects, x0 and
x1. To learn what these auxiliary ParameterObjects control, the command TIdoc ma
be used:
      

**Examining Texture documentation with TIdoc**

```
pi{auto-muteHiConga}ti{b1} :: tidoc
TI: b1, TM: LineGroove
(i)nstrument        80 (csoundNative: pluckTamHats)
(b)pm               (1) name, (2) stepString {'event', 'time'}, (3)
                    parameterObject {secPerCycle}, (4) phase, (5) exponent, (6)
                    min, (7) max
(r)hythm            (1) name, (2) pulseList {a list of Pulse notations}, (3)
                    selectionString {'randomChoice', 'randomWalk',
                    'randomPermutate', 'orderedCyclic',
                    'orderedCyclicRetrograde', 'orderedOscillate'}
local (f)ield       (1) name, (2) value
local (o)ctave      (1) name, (2) value
(a)mplitude         (1) name, (2) min, (3) max
pan(n)ing           (1) name, (2) stepString {'event', 'time'}, (3)
                    parameterObject {secPerCycle}, (4) phase, (5) exponent, (6)
                    min, (7) max
au(x)iliary
      x0            iparm (0-1)
                    (1) name, (2) directionString {'upDown', 'downUp', 'up',
                    'down'}, (3) min, (4) max, (5) increment
      x1            low-pass filter frequency
                    (1) name, (2) directionString {'upDown', 'downUp', 'up',
                    'down'}, (3) min, (4) max, (5) increment
texture (s)tatic
      s0            (1) name, (2) transpositionList, (3) timeDelay
      s1            (1) name, (2) selectionString {'randomChoice', 'randomWalk',
                    'randomPermutate', 'orderedCyclic',
                    'orderedCyclicRetrograde', 'orderedOscillate'}
      s2            (1) name, (2) level {'set', 'event'}
      s3            (1) name, (2) level {'set', 'event'}
texture (d)ynamic   none
```

Assuming that Csound is properly configured, a new set of EventLists can be
created. As the user is now in EventMode csoundNative and has csoundNative
textures, both a Csound score and a MIDI file are created. (See for more
information on working with Csound in athenaCL.) The user may render the Csound
score with ELr, and then audition the results with the ELh command.
      

**Creating a new EventList with ELn**

```
pi{auto-muteHiConga}ti{b1} :: eln
      EventList ath2010.07.03.18.40.11 complete:
/Volumes/xdisc/_scratch/ath2010.07.03.18.40.11.bat
/Volumes/xdisc/_scratch/ath2010.07.03.18.40.11.csd
/Volumes/xdisc/_scratch/ath2010.07.03.18.40.11.mid
/Volumes/xdisc/_scratch/ath2010.07.03.18.40.11.xml
audio rendering initiated: /Volumes/xdisc/_scratch/ath2010.07.03.18.40.11.bat
EventList hear initiated: /Volumes/xdisc/_scratch/ath2010.07.03.18.40.11.aif
EventList hear initiated: /Volumes/xdisc/_scratch/ath2010.07.03.18.40.11.mid
```




## Displaying Texture Parameter Values

It is often useful to view the values produced by a Texture with a graphical
diagram. The command TImap provides a multi-parameter display of all raw values
input from ParameterObjects into the Texture. The values displayed by TImap are
pre-TextureModule, meaning that they are the raw values produced by the
ParameterObjects; the final parametric event values may be altered or completely
changed by the Texture's internal processing (its TextureModule) to produce
different arrangements of events. The TImap command thus only provides a partial
representation of what a Texture produces.
      
To view a TImap display, the user's graphic preferences must be properly set
(see for more information). The command TImap displays the active Texture:
      

**Viewing a Texture with TImap**

```
pi{auto-muteHiConga}ti{b1} :: timap
TImap (event-base, pre-TM) display complete.
```
