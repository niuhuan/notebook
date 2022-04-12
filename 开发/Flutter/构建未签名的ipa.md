Flutter - 构建未签名的ipa.md
==========================

## 构建

```shell
flutter build ios --release --no-codesign
```

## 精简 (Optional)

裁剪掉flutter中x86_64的部分, 极大的减少ipa体积

```shell
foreachThin(){
  for file in $1/*
  do
      if test -f $file
      then
           mime=$(file --mime-type -b $file)
           if [ "$mime" == 'application/x-mach-binary' ]  || [ "${file##*.}"x = "dylib"x ]
           then
                echo thin $file
                xcrun -sdk iphoneos lipo "$file" -thin arm64 -output "$file"
                xcrun -sdk iphoneos bitcode_strip "$file" -r -o  "$file"
                strip -S -x "$file" -o "$file"
           fi
      fi
      if test -d $file
      then
          foreachThin $file
      fi
  done
}

foreachThin build/ios/iphoneos/Runner.app
```

## 打包

```shell
mkdir -p Payload
mv build/ios/iphoneos/Runner.app Payload
zip -9 nosign.ipa -r Payload
```



