---
title: "Float Conversion"
description: "Creating a snippter of a function to convert a 4 byte int into a float"
pubDate: "October 4, 2019"
heroImage: "/img/image-9-3255590291.jpeg"
author: Kion
---

```javascript
"readFloatLE" : function(offset, fixed) {

		var uint32 = this.readUInt32LE(offset);

		var negative = uint32 >> 31;
		var exponent = (uint32 >> 23) & 0xFF;
		var mantissa = uint32 & 0x007fffff;

		if(exponent === 0 && mantissa === 0) {
			return negative ? -0 : 0;
		} else if(exponent === 0xff && mantissa === 0) {
			return negative ? Number.NEGATIVE_INFINITY : Number.POSITIVE_INFINITY;
		} else if(exponent === 0xff && mantissa !== 0) {
			return Number.NaN;
		}

		negative = negative ? -1 : 1;
		exponent -= 127;
		exponent = Math.pow(2, exponent);

		var man = 1.0;
		var shift = 0;

		for(let i = 22; i >= 0; i--) {
			
			shift++;
			if(mantissa & (1 << i)) {
				man += 1 / (1 << shift);
			}

		}
	
		var float = negative * exponent * man;

		if(fixed) {
			float = float.toFixed(fixed);
		}

		return float;

	},
```