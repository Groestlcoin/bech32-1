BECH32(1) - General Commands Manual

# NAME

**bech32** - Encode and decode Bech32 strings and groestlcoin Segwit addresses

# SYNOPSIS

General-purpose encoding and decoding:

**bech32**
**-e**
**-h**&nbsp;*hrp*
*hexdata*  
**bech32**
**-e**
**-5**
**-h**&nbsp;*hrp*
*base32data*  
**bech32**
**-d**
\[**-5**]
\[**-u**]
*bech32string*

For Groestlcoin
**Bech32**
addresses
("grs1"):

**bech32**
**-s**&nbsp;*witver*
*hexdata*  
**bech32**
**-S**
*grs1addr*

For .onion domains:

**bech32**
**-e**
*base32domain.onion*  
**bech32**
**-d**
*onion1bech32*

# DESCRIPTION

The
**bech32**
utility is a powerful general-purpose tool for encoding and decoding
of BIP 173 standard Bech32 strings and Groestlcoin Bech32 addresses.

Hexadecimal data may be input with or without a leading
"0x"
prefix.
It is output with such a prefix, to prevent ambiguity.

It has the following modes, some of which overlap:

**-e**

> Encoder mode.
> Output is a Bech32 string.

**-d**

> Decoder mode.
> Output is the input string
> 's human-readable part
> (HRP),
> followed by a delimiting colon
> (":"),
> followed by the
> "data part"
> data in hexadecimal with a
> "0x"
> prefix.

**-s** *witver*

> Encode a Groestlcoin Segwit address with witness version
> *witver*.

**-S**

> Decode a Groestlcoin Segwit address.
> Output its witness version in
> *decimal*
> (0&#8211;16 inclusive),
> followed by a delimiting colon
> (":"),
> followed by the address data in
> *hexadecimal*
> with a
> "0x"
> prefix.

(.onion autodetect)

> Dot-onion mode.
> When encoding,
> **bech32**
> will automatically detect a dot-onion domain and read its second level
> as RFC 4648 Base32 data.
> When decoding,
> **bech32**
> will detect a
> "onion"
> HRP, and output a dot-onion domain.

The options are as follows:

**-h** *hrp*

> General encoding only, and required therefor.
> Provide the Human-Readable Portion
> (HRP)
> for the Bech32 string.

**-5**

> Read RFC 4648 Base32 data when encoding, or write RFC 4648 Base32 data
> when decoding.

**-u**

> (Decoding only.)
> Output hexadecimal characters in uppercase.

# EXIT STATUS

The **bech32** utility exits&#160;0 on success, and&#160;&gt;0 if an error occurs.

# EXAMPLES

Extract the witness version and Hash160 from the
**bech32**
address:

	bech32 -S grs1q0xez7a2j2fsvuqyxv3k9kdutv7gqt7cr4436cs

	0:0x79b22f75525260ce0086646c5b378b679005fb03

Get a
"hello, world"
introduction to Bech32:

	bech32 -e -h hello_world 48656c6c6f2c20776f726c6421

	hello_world1fpjkcmr09ss8wmmjd3jzzwhs4ff

Generate a
"burn address"
with a Hash160 of all zeroes, which would be spendable by the same unknown
private keys as the infamous 1111111111111111111114oLvT2.
**Warning:  Do NOT send coins here:**

	bech32 -s 0 0x0000000000000000000000000000000000000000

	grs1qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqcgz463

Bech32-encode the address for Wikileaks, to add error-correcting codes:

	bech32 -e wlupld3ptjvsgwqw.onion

	onion1kt50trm0nf4jxkskpcjy74

Now, decode the address someone gave you:

	bech32 -d onion1kt50trm0nf4jxkskpcjy74

	wlupld3ptjvsgwqw.onion

# STANDARDS

The
**bech32**
utility conforms to:

*	The Bech32 standard:
	P. Wuille, G. Maxwell,
	"Base32 address format for native v0&#8211;16 witness outputs",
	2016-03-20, Bitcoin
	[BIP 173](https://github.com/bitcoin/bips/blob/master/bip-0173.mediawiki).

*	S. Josefsson,
	"The Base16, Base32, and Base64 Data Encodings",
	2006-10,
	[RFC 4648](https://tools.ietf.org/html/rfc4648).
	(For transcoding RFC standard Base32 input data and .onion domains.)

*	J. Appelbaum, A. Muffett,
	"The '.onion' Special-Use Domain Name",
	2015-10,
	[RFC 7686](https://tools.ietf.org/html/rfc7686).

# AUTHORS

The
**bech32**
utility was written by
nullius
&lt;[nullius@nym.zone](mailto:nullius@nym.zone)&gt;.

PGP:
`0xC2E91CD74A4C57A105F6C21B5A00591B2F307E0C`

Tips:
[3NULL3ZCUXr7RDLxXeLPDMZDZYxuaYkCnG](bitcoin:3NULL3ZCUXr7RDLxXeLPDMZDZYxuaYkCnG),
[bc1qcash96s5jqppzsp8hy8swkggf7f6agex98an7h](bitcoin:bc1qcash96s5jqppzsp8hy8swkggf7f6agex98an7h).

The internal Bech32 encoding and decoding is done by the open-source
[Bech32 reference code](https://github.com/sipa/bech32/tree/master/ref/c)
written by Pieter Wuille
(sipa)
(no affiliation with this author).

# BUGS

This code started as a quickly-made utility for personal use, and kept
growing as such things are wont to do.
The user interface is reasonably logical, given the tool
's flexibility.
However, the source code needs some substantial refactoring.

This manpage could use more and better examples, plus some other general
editing.

Test cases are needed.
Unfortunately, the Bech32 standard does not currently provide full roundtrip
test vectors for arbitrary Bech32 strings.

Special support is planned for a concept which this author calls
"**PGP Descriptors**".
However, a spec must be drawn before releasing such a thing into the wild.

# SECURITY CONSIDERATIONS

This is an early release, which should be considered alpha-quality software.
It
**should not**
be used on untrusted inputs, such as anything blindly accepted by a webserver.
High on the author
's TODO list is to beef up input validation.
At this time, aside from a few simple checks, the utility will happily
pass the buck to the Bech32 reference functions.

Groestlcoin - December 31, 2017
