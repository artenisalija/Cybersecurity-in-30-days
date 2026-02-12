# Day 5 OhSINT - TryHackMe Challenge

**Completed:** February 12, 2026

## Challenge Overview
Open Source Intelligence (OSINT) challenge where you analyze an image file to gather information about a target using publicly available data.

## Tools Used
- **ExifTool** (web-based) - Metadata extraction
- **Google Search** - Social media discovery
- **Wigle.net** - WiFi network geolocation
- **Browser DevTools** - Source code inspection

## Solution Summary

### Initial Analysis
Started with an image file and extracted metadata using ExifTool, which revealed the copyright owner's handle in the metadata.

### Information Gathered

**Target:** OWoodflint

**Social Media Footprint:**
- **Twitter/X:** Found profile with cat avatar
- **GitHub:** Discovered email and location
- **Personal Blog:** WordPress site with additional details

### Answers Found

1. **Avatar:** Cat
2. **Location:** London
3. **WiFi SSID:** UnileverWifi (found via BSSID lookup on Wigle.net)
4. **Email:** OWoodflint@gmail.com
5. **Email Source:** GitHub
6. **Holiday Destination:** New York
7. **Password:** pennYDr0pper.! (leaked in blog source code)

## Key Takeaways
- Metadata contains valuable information often overlooked
- People frequently reuse usernames across platforms
- Source code inspection can reveal sensitive data
- BSSID information can be geolocated using specialized databases
- OSINT relies on connecting information from multiple sources

## Methodology
1. Extract metadata from provided files
2. Search for unique identifiers (usernames, handles)
3. Check common platforms (social media, code repositories, blogs)
4. Use specialized tools for technical data (BSSID lookups)
5. Inspect source code for hidden information

---

*Challenge completed as part of TryHackMe OSINT learning path*