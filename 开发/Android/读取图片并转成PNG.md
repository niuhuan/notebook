
```kotlin
    private fun convertToPNG(path: String): ByteArray {
        BitmapFactory.decodeFile(path)?.let { bitmap ->
            val maxWidth =
                    when {
                        Build.VERSION.SDK_INT >= Build.VERSION_CODES.R -> windowManager.currentWindowMetrics.bounds.width()
                        Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN_MR1 -> {
                            val displayMetrics = DisplayMetrics()
                            windowManager.defaultDisplay.getRealMetrics(displayMetrics)
                            displayMetrics.widthPixels
                        }
                        else -> throw Exception("not support")
                    }
            if (bitmap.width > maxWidth) {
                val newHeight = maxWidth * bitmap.height / bitmap.width
                val newImage = Bitmap.createScaledBitmap(bitmap, maxWidth, newHeight, true)
                return compressBitMap(newImage)
            }
            return compressBitMap(bitmap)
        }
        throw Exception("error pic")
    }

    private fun compressBitMap(bitmap: Bitmap): ByteArray {
        val bos = ByteArrayOutputStream()
        bos.use { bos ->
            Log.d("BITMAP", bitmap.width.toString())
            bitmap.compress(Bitmap.CompressFormat.PNG, 100, bos)
        }
        return bos.toByteArray()
    }
```