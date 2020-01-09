PyXbSymbolDatabase ![CI Status](https://github.com/mborgerson/PyXbSymbolDatabase/workflows/Tests/badge.svg?branch=master)
==================

Python bindings to the [Xbox Symbol
Database](https://github.com/Cxbx-Reloaded/XbSymbolDatabase). Useful for
scanning Xbox Executables (XBEs) for known XDK symbols using pattern matching.

Example Usage
-------------

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
