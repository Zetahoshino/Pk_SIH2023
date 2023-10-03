Customoptionsviewgroup

package com.example.myapplication

import android.content.Context
import android.util.AttributeSet
import android.view.Gravity
import android.widget.LinearLayout

class CustomOptionsViewGroup @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : LinearLayout(context, attrs, defStyleAttr) {

    init {
        orientation = VERTICAL
        gravity = Gravity.CENTER
    }
}
mainactivity
package com.example.myapplication

import android.app.Activity
import android.graphics.Color
import android.os.Bundle
import android.view.Gravity
import android.widget.Button
import android.widget.FrameLayout
import android.widget.Toast

class MainActivity : Activity() {
    private lateinit var homepageLayout: FrameLayout
    private lateinit var optionsLayout: FrameLayout

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        initializeHomepage()
    }

    private fun initializeHomepage() {
        homepageLayout = FrameLayout(this).apply {
            layoutParams = FrameLayout.LayoutParams(
                FrameLayout.LayoutParams.MATCH_PARENT,
                FrameLayout.LayoutParams.MATCH_PARENT
            )
            setBackgroundColor(Color.WHITE)

            val homepageText = Button(context).apply {
                text = "Welcome to the App!"
                textSize = 24f
                setTextColor(Color.BLACK)
                layoutParams = FrameLayout.LayoutParams(
                    FrameLayout.LayoutParams.WRAP_CONTENT,
                    FrameLayout.LayoutParams.WRAP_CONTENT
                ).apply {
                    gravity = Gravity.CENTER
                }
            }
            val nextButton = Button(context).apply {
                text = "Next"
                textSize = 20f
                layoutParams = FrameLayout.LayoutParams(
                    FrameLayout.LayoutParams.WRAP_CONTENT,
                    resources.getDimensionPixelSize(R.dimen.button_height)
                ).apply {
                    gravity = Gravity.BOTTOM or Gravity.END
                }

                setOnClickListener {
                    showOptions()
                }
            }
            addView(homepageText)
            addView(nextButton)

            setContentView(this)
        }
    }

    private fun showOptions() {
        optionsLayout = FrameLayout(this).apply {
            layoutParams = FrameLayout.LayoutParams(
                FrameLayout.LayoutParams.MATCH_PARENT,
                FrameLayout.LayoutParams.MATCH_PARENT
            )
            setBackgroundColor(Color.WHITE)

            val options = listOf(
                Pair("Network Analysis", "#4285F4"),
                Pair("Behavioral Analysis", "#0F9D58"),
                Pair("Threat Reports", "#DB4437"),
                Pair("Previous Scan Report", "#F4B400")
            )

            val buttonHeight = resources.getDimensionPixelSize(R.dimen.button_height)
            var marginTop = 0

            for (option in options) {
                val customButton = Button(this@MainActivity).apply {
                    text = option.first
                    setBackgroundColor(Color.parseColor(option.second))
                    setTextColor(Color.WHITE)  // Set text color to white
                    layoutParams = FrameLayout.LayoutParams(
                        FrameLayout.LayoutParams.MATCH_PARENT,
                        buttonHeight
                    ).apply {
                        gravity = Gravity.CENTER
                        topMargin = marginTop
                        marginTop += buttonHeight + 16 // 16dp spacing
                    }

                    setOnClickListener {
                        showToast("${tag} option!!")
                    }
                }
                addView(customButton)
            }

            setContentView(this)
        }
    }

    private fun showToast(message: String) {
        Toast.makeText(this, message, Toast.LENGTH_SHORT).show()
    }
}
custombutton.kt
package com.example.myapplication

import android.content.Context
import android.graphics.Canvas
import android.graphics.Color
import android.graphics.Paint
import android.util.AttributeSet
import android.view.Gravity
import android.widget.Button
import android.widget.Toast

class CustomButton @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : Button(context, attrs, defStyleAttr) {

    init {
        setTextColor(Color.WHITE)
        gravity = Gravity.CENTER
        setPadding(32, 16, 32, 16)

        setOnClickListener {
            showDevelopmentPrompt()
        }
    }

    override fun onDraw(canvas: Canvas?) {
        val paint = Paint()
        paint.color = Color.parseColor("#FF0000") // Red border color
        paint.strokeWidth = 4f
        paint.style = Paint.Style.STROKE
        canvas?.drawRect(0f, 0f, width.toFloat(), height.toFloat(), paint)
        super.onDraw(canvas)
    }

    private fun showDevelopmentPrompt() {
        Toast.makeText(context, "App is under development", Toast.LENGTH_SHORT).show()
    }
}
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <dimen name="button_height">48dp</dimen>
</resources>
