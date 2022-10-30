# pyPetKit

## Purpose:

Connects to PetKit cloud API servers to communicate with **PetKit Feeder Mini**, **PetKit Fresh Element 3** Smart Pet Feeders, as well as the **PuraX Automated Cat Litter System**. Additional device support will be provided either if I  acquire more PetKit devices for my home, or if others own PetKit products that are not yet covered, and are willing to collaborate. 

Obtain status of feeders (including whether food reserves are getting low, or the feeding bowl is over-full), pet feeding schedules, pet eating patterns, and dispense additional "snack" bursts of food.

With the PuraX litter system, obtain the status, litter remaining, warnings (eg: waste bin needs emptying, cat is currently near/in the litterbox, etc), and retrieve the "pet visiting patterns" *(see futher below)*. Trigger an odour removal, cleaning cycle, or remote power-off/power-on of the unit and update the self-cleaning and deodourising schedule (note: a number of these I've got all the API information, but not completed coding in full - the above documents the current work that I'm doing with the code).

If you accuratelty record your cat's weight for each cat in the PetKit app (and they have distinct weights, I aim to match the pet up with each "visit" to the PuraX litterbox - inspired by the work of [@lydian](lydian/petkit_pura_x_exporter), but expected to be implemented afresh).

Testing performed on the **PetKit Fresh Element 3** Smart Pet Feeder, as well as the **PuraX Automated Cat Litter System**.

### Warning: ###

I've reverse-engineered a number of smart-home products, and to date, this is the first I've seen that sends 99% of its communications between PetKit's mobile apps and its API servers in plain-text using HTTP!

While your account password is hashed using MD5 before being sent across the Internet, no other personal info is.

Additionally, BEWARE if you allow the mobile app permissions to access location information - it sends this embedded in the HTTP header of __EVERY API CALL__ it makes to its servers - completely unencrypted! With the iOS app, even revoking location permissions doesn't stop it from sending the exact latitude/longitude of the last place it was able to get a position fix on you. 

Information such as your wireless network SSID and BSSID (MAC address of your router), your email address, your location, and anything you disclose about yourself or your pets are all flying around unsecured (so I cringe to think what the security at both the server and device firmware ends are like).

This library obviously isn't stealthily going to be sending ongoing location data to PetKit servers, but the library can only communicate with PetKit servers using the same methods as their native smart device apps (which is HTTP, and exchanges a surprising amount of information - including BACK FROM THE PETKIT SERVERS once you've configured your own devices using their native app).  

This library will never be a substitute for using the PetKit official app(s), but I provide you with fair warning that the security protecting your information is pretty weak!
### Usage: ###

```python
from pypetkit import PetKitAPI
from pprint import pprint

petkit_api = PetKitAPI(<<YOUR_USERNAME>>,<<YOUR_PASSWORD>>
   [, <<API_COUNTRY_CODE>> <<API_LOCALE_CODE>>, <<API_TIMEZONE>>])
```

#### Authenticate with Petoneer API ####

```python
petkit_api.request_token()
```

#### Obtain List of PetKit Devices registered to your account ####

```python
petkit_api.get_all_devices()
feeders = petkit_api.get_sensors()

pprint(feeders)
```

## API Documentation:

As I'm able to capture MitM API calls between the iOS PetKit app and their API servers, I'm documenting all discovered API endpoints and extrapolating what certain values mean within both status reads and property posts to the system.

The latest version of the API documentation (as an ongoing Work-In-Progress) can be found at one of the following links.  

*___PLEASE Note:___ These are in no way offical documentation of the PetKit API's, and I can't be responsible for any breaking changes that PetKit makes, nor any data loss (or data breach given that PetKit doesn't impmenet HTTPS communications on their main API servers!).*

 - [PetKit (D3) Fresh Element 3 Pet Feeder API 
Methods](https://bit.ly/PetKit_API_D3)

 - [PetKit (T3) PuraX Pet Litterbox API Methods](https://bit.ly/PetKit_API_T3)


Collaboration is welcomed if you own other PetKit products, and are happy to either contribute code, or would be happy to capture info relating to device-specific API calls (using a "man-in-the-middle" local web proxy). 

My aim is to provide my new code (and evolving API documentation) back to the original library author ([@CrazYoshi](https://github.com/CrazYoshi)) - to either integrate or pass over as suits this author's vision for their library.

## Credit:
This library is forked from the initial [[pyPetKit](https://github.com/CrazYoshi/pyPetKit)] library, created by [@CrazYoshi](https://github.com/CrazYoshi). 

Additional capabilities are being added to library to cater for the PetKit PuraX Automated Litterbox, and the PetKit Fresh Element 3 Pet Feeder. 

I aim to improve the modularity of this library to prepare its API abstraction for use within a [Home Assistant](https://www.home-assistant.io/) smart home ([HACS](https://hacs.xyz/)) integration add-on (\* *COMING SOON* \*).
## License

This project is licensed under the GPLv3 License
