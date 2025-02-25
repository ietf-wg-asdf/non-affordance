---
v: 3
docname: draft-lee-asdf-digital-twin-latest
cat: std
submissiontype: IETF
pi:
  docmapping: 'yes'
  toc: 'true'
  symrefs: 'true'
  sortrefs: 'true'

title: Semantic Definition Format (SDF) modeling for Digital Twin
abbrev: SDF modeling for digital twin
area: ART
wg: ASDF Working Group
kw: Internet-Draft

author:
- role: editor
  name: Hyunjeong Lee
  org: Electronics and Telecommunications Research Institute
  abbrev: ETRI
  street: 218 Gajeong-ro, Yuseong-gu
  city: Daejeon
  code: '34129'
  country: South Korea
  phone: "+82 42 860 1213"
  email: hjlee294@etri.re.kr
- name: Jungha Hong
  org: Electronics and Telecommunications Research Institute
  abbrev: ETRI
  street: 218 Gajeong-ro, Yuseong-gu
  city: Daejeon
  code: '34129'
  country: South Korea
  phone: "+82 42 860 0926"
  email: jhong@etri.re.kr
- ins: J-S. Youn
  name: Joo-Sang Youn
  org: DONG-EUI University
  abbrev: Dong-eui Univ
  street: 176 Eomgwangno Busan_jin_gu
  city: Busan
  code: '47340'
  country: South Korea
  phone: "+82 51 890 1993"
  email: joosang.youn@gmail.com
- ins: Y-G. Hong
  name: Yong-Geun Hong
  org: Daejeon University
  street: 62 Daehak-ro, Dong-gu
  city: Daejeon
  code: '34520'
  country: South Korea
  phone: "+82 42 280 4841"
  email: yonggeun.hong@gmail.com
normative:
  Y.4600:
    title: "\"Recommendation ITU-T Y.4600 (2022), Requirements and capabilities of
      a digital twin system for smart cities."
    author:
    - org: International Telecommunication Union
    date: 2022-08
  I-D.ietf-asdf-sdf:
  I-D.laari-asdf-relations:
  ISO23247-1:
    target: https://www.iso.org/standard/75066.html
    title: 'Automation systems and integration Digital twin framework for manufacturing
      - Part 1: Overview and general principles, ISO 23247-1.'
    author:
    - org: ISO
    date: 2021-10
  ISO23247-3:
    target: https://www.iso.org/standard/78744.html
    title: 'Automation systems and integration Digital twin framework for manufacturing
      - Part 3: Digital representation of manufacturing elements, ISO 23247-3.'
    author:
    - org: ISO
    date: 2021-10
informative:
  saref4bldg:
    target: https://saref.etsi.org/saref4bldg
    title: SAREF extension for building
    author:
    - name: Mara Poveda-Villaln
    - name: Ral Garcia-Castro
    date: '2020-06-05'

--- abstract


This memo specifies SDF modeling for a digital twin,
i.e. a digital twin system, and its interactions.
An SDF is a format to describe Things and their associated interactions,
and to represent the various kinds of information that is exchanged for these
interactions.
Therefore, the SDF format can be used to define the behavior of things, i.e.
physical objects,
and related data and interaction models in a digital twin that contain objects
as components.

--- middle

# Introduction

A digital twin is defined as a digital representation of an object of interest
and
may require different capabilities, for example, synchronization and real-time
support,
according to the specific domain of application. {{Y.4600}}.
Digital twin help organizations improve important functional objectives,
including real-time control, off-line analytics, and predictive maintenance,
by modeling and simulating objects in the real world.
Therefore, it is important for a digital twin to represent
as much real-world information about the object as possible when digitally
representing the object.

Nowadays, digital twin technologies are applied in various domains including
manufacturing, energy, medical, farm, transportation, etc.
And a common format is needed to represent the objects in the domains as
digital twins.
SDF {{I-D.ietf-asdf-sdf}} can be used for modeling objects as digital twins.

This document specifies the modeling and guidance on how to use SDF to represent
objects as digital twins.


# Terminology {#terminology}

This specification uses the terminology specified in {{I-D.ietf-asdf-sdf}} in particular "Class Name Keyword", "Object", and "Affordance".

{::boilerplate bcp14-tagged}


# SDF structure for digital twin

This section describes SDF structure with the new Class Name Keyword, sdfNonAffordance,
to represent a thing or an object as a digital twin.

## New Element for digital twin

The SDF language uses six Class Name Keywords, and sdfNonAffordance is added
as a new Class Name Keyword for digital twin.

The information attributes for digital twin are defined in {{ISO23247-3}}.
Some of them, for example characteristics and status, can be described using
existing Class Name Keywords.
And the others, for example location, can be described using sdfNonAffordance.


~~~~ json
{
  "info": {
    "title": "An example of the heater #1 in the boat #007",
    "version": "2025-01-27",
    "copyright": "Copyright 2025. All rights reserved."
  },
  "namespace": {
    "heater1": "https://example.com/heater1"
  },
  "defaultNamespace": "heater1",
  "sdfObject": {
    "boat": {
      "sdfProperty": {
        "value": {
          "description": "The state of the heater #1 in a the boat #007; false for off and true for on.",
          "type": "boolean"
        }
      },
      "sdfNonAffordance": {
        "location": {
          "wgs84": {
            "latitude": "35.2988233791372",
            "longitude": "129.25478376484913",
            "altitude": "0.0"
          },
          "postal": {
            "city": "Ulsan",
            "post-code": "44110",
            "country": "South Korea"
          },
          "w3w": {
            "what3words": "toggle.mopped.garages"
          }
        },
        "report": {
          "value": {
            "description": "On February 24, 2025, the boat #007's heater #1 was on from 9 a.m. to 6 p.m.",
            "type": "string"
          }
        }
      }
    }
  }
}
~~~~
{: #example1 post="fold" title='An example of SDF for digital twin'}


## Architecture of Digital Twin {#ref-architecture}
{: toc="default"}

The architecture of a digital twin based on the SDF model is illustrated
in {{basic-arch-fig}},
, following the guidelines of {{ISO23247-3}}.

The Physical Layer comprises affordance and non-affordance objects. From
the real-world objects,
only those deemed relevant are selected for representation as digital twins.
The Digital Twin Layer is structured into three sublayers: the Device Communication
Sublayer,
the Digital Twin Sublayer, and the Application Sublayer.
The Device Communication Sublayer is responsible for monitoring and collecting
data
from both affordance and non-affordance objects.
This sublayer provides the necessary data to synchronize the physical objects
with their digital twin counterparts.
The Digital Twin Sublayer ensures synchronization between the affordance
and non-affordance objects
and their respective digital twins using the data provided by the Device
Communication Sublayer.
The Application Sublayer presents the synchronized values of the digital
twins to users,
facilitating informed decision-making.


~~~~
+---------------------------------------------+ - - - - - - - - - - -
|            Application Sublayer             |
| +----------+ +------+ +--------+ +--------+ |
| |  Human   | | HMI  | |  Apps  | |  Peers | |
| +----------+ +------+ +--------+ +--------+ |
+---------------------------------------------+
|           Digital Twin Sublayer             |
| +----------+ +-------------+ +------------+ |
| | Operation| | Application | | Resource   | |
| |    and   | |     and     | | access and | |
| |management| |   service   | |interchange | |
| +----------+ +-------------+ +------------+ |
| +-----------------------------------------+ |  Digital twin Layer
| |     Digital representation of objects   | |
| |   +-------------+   +----------------+  | |
| |   |  Affordance |   | Non-Affordance |  | |
| |   |   objects   |   |    objects     |  | |
| |   +-------------+   +----------------+  | |
| +-----------------------------------------+ |
+---------------------------------------------+
|        Device Communication Sublayer        |
|     +-------------+   +----------------+    |
|     |    Data     |   |     Object     |    |
|     | collection  |   |     control    |    |
|     +-------------+   +----------------+    |
+---------------------------------------------+ - - - - - - - - - - -
|     +-------------+   +----------------+    |
|     |  Affordance |   | Non-Affordance |    |
|     |   objects   |   |    objects     |    |     Physical Layer
|     +-------------+   +----------------+    |
+---------------------------------------------+ - - - - - - - - - - -
~~~~
{: #basic-arch-fig title='Basic Architecture of digital twin' artwork-align="center"}



# Requirements for digital twin
{: id="requirements-digital-twin"}

## Overview {#overview}

A digital twin is a partial representation of sdfThing or sdfObject that
contains attributes such as sdfProperty, sdfAction and sdfEvent{{ISO23247-1}}.
By representing sdfThing as a digital twin, crucial events that require appropriate
action can be quickly detected and controlled.
The requirements defined in {{ISO23247-1}} are applied to represent sdfThings and sdfObjects as digital twins.


## Data acquisition
{: id="data-acquisition"}

Data related to sdfThing and sdfObject, such as sdfProperty, sdfEvent, and
sdfAction, should be collected from IP and non-IP devices.


## Data analysis
{: id="data-analysis"}

The collected data needs to be analyzed to understand the state of sdfThing
and sdfObject.


## Identification {#identification}

The sdfThings and sdfObjects should contain data that uniquely identifies
them as digital twins.


## Accuracy {#accuracy}

The sdfThings and sdfObjects should be represented as digital twins with
appropriate levels of detail and accuracy,
depending on the application.


## Synchronization {#synchronization}

The sdfThings and sdfObjects should be synchronized with their digital twins
in real-time as appropriate for the application.
Newly added or removed sdfThings and sdfObjects should be recognized and
reflected in the digital twin.



# Procedures for digital twin
{: id="procedure-for-digital-twin"}

A procedure for representing sdfThing, as a digital twin in a domain is as
follows:



* defining a purpose for expressing the observable object, as known as a physical
  asset or an object of interest, as a digital twin in the domain

* organizing data based on the roles of the observable object in the domain

* configuring the observable object into the digital twin based on the data
  for the purposes

* interworking with a digital twin of each of other domains in which the observable
  object performs a different role

* synchronizing the observable object and the digital twin



# Security Considerations {#security-considerations}

Only authorized users should have the authority to manage sdfThings and sdfObjects.


# IANA Considerations {#iana-considerations}

This document has no IANA actions.


--- back
