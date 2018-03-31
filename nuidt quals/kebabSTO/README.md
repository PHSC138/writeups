We start with a .pcap, so our first step is to open it in Wireshark.

Started by extracting some html files, but I didn't find anything interesting..

Follow TCP stream is pretty good, we see a mail conversation and a file being sent.

From stream 5 we see:
"The name of the file begins with "kd"
and
"they also found a service at mydomainndh.ndh (port 55555) which decrypts every text encrypted with the public key, apart from the interesting one"

In stream 13 we get the file "docs.zip"

UEsDBAoAAAAAAFZ6lUsAAAAAAAAAAAAAAAAFABwAZG9jcy9VVAkAA8PCO1rdwjtadXgLAAEE
AAAAAAQAAAAAUEsDBBQAAAAIALd5lUvQbcUR4wAAABABAAAPABwAZG9jcy9wdWJrZXkucGVt
VVQJAAOawTtamsE7WnV4CwABBAAAAAAEAAAAAGXPvVaDMABA4Z2n6M7hlAoKjEmIIWDTRsQD
bBESaKAHqBZant6f1Tve7bOsnyAmlG2OGXyhaJPg4vdZxp4StQc2QelEUvrhhBxDwDMAXMJA
iOCJJ7DhyI9Zy6iQHS91tfO3c1HK3dO8xEZeTcr/fNflBXHlicfbpSZjdOg7hL8EVolIUG76
7EwCoO/SC1yXVlHkyb7QrT1Kaqy79PU6d+71efRANYAgdtb6EGdi3T6oocl7YS5qPca3KRig
NxdAuW+lduwwS9WY0co4iWaxdat13Y5nqbC50BBwAI0/IWbhP/Q3UEsDBBQAAAAIAFZ6lUus
24QeoQAAADUBAAAPABwAZG9jcy9jaXBoZXJUZXh0VVQJAAPDwjtaw8I7WnV4CwABBAAAAAAE
AAAAABWPyREAQQgC/xuNFwL5J7bOS6pstWWJTYxoe/3qYopgqq6OQruta0ieSC8nAmvPMGJU
dmics9wyyrkHpTRV0wVPZoYboT5GDCYmr+GHtrBhLuuGW4llFBXLTFgBbJPOEurBXQ6c0m1c
xGlI0ZF9wq0L1SeoSkY/n+jtPLcLieaBhiYBAtmsZutOmad7MwzEiXVy4+6Z991gvMPi2ewk
6/sBUEsBAh4DCgAAAAAAVnqVSwAAAAAAAAAAAAAAAAUAGAAAAAAAAAAQAO1BAAAAAGRvY3Mv
VVQFAAPDwjtadXgLAAEEAAAAAAQAAAAAUEsBAh4DFAAAAAgAt3mVS9BtxRHjAAAAEAEAAA8A
GAAAAAAAAQAAAKSBPwAAAGRvY3MvcHVia2V5LnBlbVVUBQADmsE7WnV4CwABBAAAAAAEAAAA
AFBLAQIeAxQAAAAIAFZ6lUus24QeoQAAADUBAAAPABgAAAAAAAEAAACkgWsBAABkb2NzL2Np
cGhlclRleHRVVAUAA8PCO1p1eAsAAQQAAAAABAAAAABQSwUGAAAAAAMAAwD1AAAAVQIAAAAA


base64 -d < doc > docs.zip

unzip docs.zip

we get cipherText and pubkey.pem

the mydomain... is just the url from the challenge page: kebabsto.challs.malice.fr

there was an error in the file, the port is actually 8888 and not 55555

inputted the cipherText and got 
"Here is the cleartext of your input :


123360975347216093033775350245751721746535757669936"

couldn't find anything from this so I went back to the pcap

in stream 11 we can see that there is a file being got "GET /kdsqfkpdsdf"

we save it, and it's a zip

when we unzip it, it's another tcp stream

we have a full 4 way handshake and the SSID wifiAccess so let's do some aircrack-ng

Opening lkdjflknezcz
Read 1358 packets.

   #  BSSID              ESSID                     Encryption

   1  F0:D7:AA:77:BD:46  wifiAccess                WPA (1 handshake)

Choosing first network as target.

Opening lkdjflknezcz
Reading packets, please wait...

                                 Aircrack-ng 1.2 rc4

      [00:00:00] 612/7120714 keys tested (2478.91 k/s)

      Time left: 47 minutes, 53 seconds                          0.01%

                           KEY FOUND! [ abcdefgh ]


      Master Key     : 46 DE 68 77 59 26 52 28 68 59 E3 E9 27 C2 75 66
                       77 A0 C0 C2 59 7C B7 6A 52 06 A3 B8 5D 7F 33 29

      Transient Key  : C8 2A 89 4B 43 93 57 73 35 B7 9E 21 99 8A 5A F2
                       B6 89 B8 10 F6 AF 77 68 A8 B4 69 E7 30 E4 A7 9B
                       88 32 93 FF AA B5 8E CE 9E AC 4A 05 05 0C EC BB
                       37 C9 12 11 5B DA 0C E9 D8 25 02 5E F3 D2 AA 4F

      EAPOL HMAC     : 76 32 AE BA 65 FD A2 64 BD FD 8E 76 BA 1F B7 84
found the key very quick!!
add to the 802.11 decryption keys along with the SSID (wifiAccess)

abcdefgh:wifiAccess

packet 1292 there's another zip file, save as raw to "slkfdsfljkj"
tried to unzip and it's asking for a password...

This is when the CTF ended...



