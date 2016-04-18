# Openwrt GSM
Light weight gsm module for [Openwrt](https://openwrt.org)

# Example

USDD
```py
#!/usr/bin/env python

PORT = '/dev/ttyUSB1' # Device 
BAUDRATE = 115200
USSD_STRING = '*5000*500#' # USSD code

from openwrt_gsm.modem import GsmModem
from openwrt_gsm import pdu

def main():
    print('Initializing modem...')
    logging.basicConfig(format='%(levelname)s: %(message)s', level=logging.DEBUG)
    modem = GsmModem(PORT, BAUDRATE)
    modem.connect(None)
    modem.waitForNetworkCoverage(10)
    response = modem.sendUssd(USSD_STRING)
    _response = iter(pdu.toByteArray(response.message))
    result = unicode(pdu.decodeUcs2(_response, _response))
    print "Reply recieved:"
    print result
    if response.sessionActive:
        print 'Closing USSD session.'
        response.cancel()
    else:
        print 'USSD session was ended by network.'
    modem.close()

if __name__ == '__main__':
    main()
```

[More Examples](https://github.com/faucamp/python-gsmmodem/tree/master/examples)

# Credits
[Python GSM Modem](https://github.com/faucamp/python-gsmmodem)
