# FlightPadFMC 737 – PMDG small-font support (85/65)

The Windows server already transmits every PMDG CDU cell with JSON fields `s` (character), `c` (colour), `f` (formatting flags). Verified from a Wireshark capture on port 8765. Android previously ignored `f`.

This change reads `f` via `optInt` (default 0), passes it to CDUView, and renders bit 0x01 as small text. Normal text uses 85% of a cell's height; small text uses 65%. The grid is 24 columns x 14 rows; existing coordinates are preserved. Actual readability needs verification on an Android 7.1 tablet.

The **debug APK** uses application ID `com.virtualfmc.fmc737.smallfont` and label **737 CDU Small Font**, so it can coexist with the author's signed APK. Wire-protocol version stays 0.4.1 for compatibility. Version code is 5. The Windows server remains unchanged.

After a successful GitHub Actions build, download the APK directly from the repo's GitHub **Releases** page (tag `smallfont-v0.4.1-r1`), or via its Actions artifact ZIP. Do not download `FMC737/app/release/737PMDG.apk`: this is the original prebuilt APK.

Build prerequisites: Android SDK platform 34, Android build-tools 34.0.0, Java 17, Gradle 7.6.4 (Android Gradle Plugin 7.4.2). From `FMC737/`, run `gradle :app:assembleDebug`.

Only `SMALL_FONT` bit 0x01 is rendered. The other bits are stored but not rendered. This build has not yet been tested on a physical tablet.
