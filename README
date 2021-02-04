# SS58 

SS58 is a simple address format designed for Substrate based chains. There's no problem with using other address formats for a chain, but this serves as a robust default. It is heavily based on Bitcoin's Base-58-check format with a few alterations.

The basic idea is a base-58 encoded value which can identify a specific account on the Substrate chain. Different chains have different means of identifying accounts. SS58 is designed to be extensible for this reason.

Basic Format
The basic format conforms to:

base58encode ( concat ( <address-type>, <address>, <checksum> ) )
That is, the concatenated byte series of address type, address and checksum then passed into a base-58 encoder. The base58encode function is exactly as defined in Bitcoin and IPFS, using the same alphabet as both.

Address Type
The <address-type> is one or more bytes that describe the precise format of the following bytes.

Currently, there exist several valid values:

00000000b..=00101111b (0..=63 inclusive): Simple account/address/network identifier. The byte can be interpreted directly as such an identifier.
01000000b..=01111111b (64..=127 inclusive) Full address/address/network identifier. The low 6 bits of this byte should be treated as the upper 6 bits of a 14 bit identifier value, with the lower 8 bits defined by the following byte. This works for all identifiers up to 2**14 (16,383).
10000000b..=11111111b (128..=255 inclusive) Reserved for future address format extensions.
The latter (42) is a "wildcard" address that is meant to be equally valid on all Substrate networks that support fixed-length addresses. For production networks, however, a network-specific version may be desirable to help avoid the key-reuse between networks and some of the problems that it can cause. Substrate Node will default to printing keys in address type 42, though alternative Substrate-based node implementations (e.g. Polkadot) may elect to default to some other type.

Address Formats for Substrate
There are 16 different address formats, identified by the length (in bytes) of the total payload (i.e. including the checksum).

3 bytes: 1 byte account index, 1 byte checksum
4 bytes: 2 byte account index, 1 byte checksum
5 bytes: 2 byte account index, 2 byte checksum
6 bytes: 4 byte account index, 1 byte checksum
7 bytes: 4 byte account index, 2 byte checksum
8 bytes: 4 byte account index, 3 byte checksum
9 bytes: 4 byte account index, 4 byte checksum
10 bytes: 8 byte account index, 1 byte checksum
11 bytes: 8 byte account index, 2 byte checksum
12 bytes: 8 byte account index, 3 byte checksum
13 bytes: 8 byte account index, 4 byte checksum
14 bytes: 8 byte account index, 5 byte checksum
15 bytes: 8 byte account index, 6 byte checksum
16 bytes: 8 byte account index, 7 byte checksum
17 bytes: 8 byte account index, 8 byte checksum
34 bytes: 32 byte account id, 2 byte checksum
Checksum types
Several potential checksum strategies exist within Substrate, giving different length and longevity guarantees. There are two types of checksum preimage (known as SS58 and AccountID) and many different checksum lengths (1 to 8 bytes).

In all cases for Substrate, the Blake2-256 hash function is used. The variants simply select the preimage used as the input to the hash function and the number of bytes taken from its output.

The bytes used are always the left most bytes. The input to be used is the non-checksum portion of the SS58 byte series used as input to the base-58 function, i.e. concat( <address-type>, <address> ). A context prefix of 0x53533538505245, (the string SS58PRE) is prepended to the input to give the final hashing preimage.

The advantage of using more checksum bytes is simply that more bytes provide a greater degree of protection against input errors and index alteration at the cost of widening the textual address by an extra few characters. For the account ID form, this is insignificant and therefore no 1-byte alternative is provided. For the shorter account-index formats, the extra byte represents a far greater portion of the final address and so it is left for further up the stack (though not necessarily the user themself) to determine the best tradeoff for their purposes.

Simple/full address types and account/address/network identifiers
The table above and, more canonically, the codebase as well as the registry express the status of the account/address/network identifiers (identifiers).

Identifiers up to value 64 may be expressed in a simple format address, in which the LSB byte of the identifier value is expressed as the first byte of the encoded address. For identifiers of between 64 and 16,383, the full format address must be used, which encodes the 14-bit value as the lower 6 bits of the first byte and the full 8 bits of the second. Identifiers of 16384 and beyond are not currently supported.
