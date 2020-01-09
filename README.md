PyXbSymbolDatabase ![CI Status](https://github.com/mborgerson/PyXbSymbolDatabase/workflows/Tests/badge.svg?branch=master)
==================

Python bindings to the [Xbox Symbol
Database](https://github.com/Cxbx-Reloaded/XbSymbolDatabase). Useful for
scanning Xbox Executables (XBEs) for known XDK symbols using pattern matching.

Install
-------

You will need CMake and a compiler installed in order to build. Then install
using PIP:

```bash
python3 -m pip install --user git+https://github.com/mborgerson/PyXbSymbolDatabase
```

Example Usage
-------------

### As a command-line tool

Invoke the module with a path to an XBE file to scan the XBE for any known
symbols.

```
$ python -m XbSymbolDatabase default.xbe
    D3D8    0    1 00192390 D3DDEVICE
    D3D8    0    1 0018f57c D3DRS_CULLMODE
    D3D8    0    1 0018f4c8 D3DDeferredRenderState
    D3D8    0    1 0018f180 D3DDeferredTextureState
    D3D8    0    1 0018f5c0 g_Stream
  DSOUND 3911    8 001946ea CDirectSoundStream_Constructor
  DSOUND 3911    8 00194358 CDirectSoundStream_AddRef
  DSOUND 3911    8 00194368 CDirectSoundStream_Release
  DSOUND 3911    8 001937aa CDirectSoundStream_GetInfo
  DSOUND 3911    8 0019384f CDirectSoundStream_GetStatus
  DSOUND 3911    8 00193884 CDirectSoundStream_Process
  DSOUND 3911    8 001937f5 CDirectSoundStream_Discontinuity
  DSOUND 3911    8 00193822 CDirectSoundStream_Flush
 XAPILIB 3911   40 00012d01 CreateMutex
 XAPILIB 3911   40 00012be3 CreateThread
 XAPILIB 3911   40 00014d39 MoveFileA
 XAPILIB 3911   40 00012aa3 SwitchToThread
 XAPILIB 3911   40 00015ba5 XCalculateSignatureBegin
 XAPILIB 3911   40 00018b0c XapiBootDash
 XAPILIB 3911   40 00018b71 XapiInitProcess
[...]
```

### As a library

Import the `XbSymbolScan` function from the `XbSymbolDatabase` module and give
it your XBE data.

```python
#!/usr/bin/env python
import argparse
from XbSymbolDatabase import XbSymbolScan

def main():
	p = argparse.ArgumentParser()
	p.add_argument('xbefile')
	args = p.parse_args()

	with open(args.xbefile, 'rb') as f:
		xbe = f.read()
		for lib, flags, sym, addr, ver in XbSymbolScan(xbe):
			print('%8s %4d %4x %08x %s' % (
				str(lib, encoding='ascii'), ver, flags, addr,
				str(sym, encoding='ascii')))
```
