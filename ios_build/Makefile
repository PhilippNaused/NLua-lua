.PHONY: all

all: ios tvos maccatalyst

ios: ./ios/liblua55.framework/liblua55
./ios/liblua55.framework/liblua55:
	xcodebuild  -project lua.ios.xcodeproj -configuration Release -sdk iphonesimulator
	xcodebuild  -project lua.ios.xcodeproj  -configuration Release -sdk iphoneos
	ditto ./Build/Release-iphoneos/ ./ios/
	rm ./ios/liblua55.framework/liblua55
	lipo -create ./build/Release-iphonesimulator/liblua55.framework/liblua55  ./build/Release-iphoneos/liblua55.framework/liblua55 -output  ./ios/liblua55.framework/liblua55

tvos: ./tvos/liblua55.framework/liblua55
./tvos/liblua55.framework/liblua55:
	xcodebuild  -project lua.tvos.xcodeproj -configuration Release -sdk appletvsimulator
	# Xcode 12 has a bug where it adds arm64 to appletv simulator
	./fix_tvos.sh
	xcodebuild  -project lua.tvos.xcodeproj -configuration Release -sdk appletvos
	ditto ./Build/Release-appletvos/ ./tvos/
	rm ./tvos/liblua55.framework/liblua55
	lipo -create ./build/Release-appletvsimulator/liblua55.framework/liblua55  ./build/Release-appletvos/liblua55.framework/liblua55 -output  ./tvos/liblua55.framework/liblua55

maccatalyst: ./maccatalyst/liblua55.framework/liblua55
./maccatalyst/liblua55.framework/liblua55:
	xcodebuild -configuration Release -destination "platform=macOS,variant=Mac Catalyst" -project lua.maccatalyst.xcodeproj -scheme liblua55 SYMROOT=build_maccatalyst/
	ditto ./build_maccatalyst/Release-maccatalyst/liblua55.framework/Versions/A/ ./maccatalyst/liblua55.framework/
	# Xcode has a bug that ignores -install_name setting and always put the /Versions/A/liblua55 for maccatalyst
	install_name_tool -id @rpath/liblua55.framework/liblua55 maccatalyst/liblua55.framework/liblua55
	security find-identity -v -p codesigning
	/usr/bin/codesign --force --sign C1279B86F04B0A2A68FAF243D0E8E669575CA0C2 -o runtime --timestamp\=none --generate-entitlement-der ./maccatalyst/liblua55.framework/liblua55

clean:
	rm -Rf build
	rm -Rf ./ios/
	rm -Rf ./tvos/
	rm -Rf ./maccatalyst/