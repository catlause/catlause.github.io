---
layout: page
title: coding pitfall
tagline: at least avoid 2nd fall
description: Record my bugs
---

## dangerous goto

original
```c
	}
	return LH_EXCEPTION; // unreachable

BADNEXTRESTORE:
	apr_off_t offset = bufpos+bufsize;
	stcode = apr_file_seek(fc->cachefp, APR_SET, &offset);
	if (APR_SUCCESS != stcode) {
		return LH_EXCEPTION;
	}
	return LH_BADNEXT;
}
```
compile error: `a label can only be part of a statement and a declaration is not a statement`

buggy edit
```c
    apr_off_t offset = bufpos+bufsize;
BADNEXTRESTORE:
    stcode = apr_file_seek(fc->cachefp, APR_SET, &offset);
```

debug edit
```c
    apr_off_t offset;
BADNEXTRESTORE:
    offset = bufpos+bufsize;
	stcode = apr_file_seek(fc->cachefp, APR_SET, &offset);
```
