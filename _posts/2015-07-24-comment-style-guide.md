---
layout: post
title: Comment Style Guide
---

#### Basic Notes

* Comment at least each class and each public and protected member of a class.
* Comment also every argument in a function.
* Overridden functions need not be documented again, but indicate where the orginial function comes from (e.g., `public cppexpose::Object interface`).
* Use active form for descriptions, do not use third person (e.g., 'Get list of functions', not: 'Returns list of functions').
* Always indicate whether pointers can be null or not.
* Always indicate the value range and/or units of arguments, return values and variables (e.g., 'Delay in seconds', not just 'Delay').
* Add explanations and word definitions whenever necessary to clarify the overall software architecture. For example, a comment like 'Get glyph decorator' does not help much if it is unclear what a glyph decorator does exactly. So, explain it in the description of the class!


#### Example/Template

{% comment %}

	data-line="10-18,22,26-32,35-38,41-54,59-67,70-83,86-96,101,106-107"
    
    data-comment-line-12="Short description"
    data-comment-line-14="Add a detailed description with examples if necessary"
    data-comment-line-22="Document variables directly after their declaration"
    data-comment-line-28="Short description"
    data-comment-line-30="[in]/[out]/[in,out], argument name"
    data-comment-line-31="Indicate if nullptr is allowed for pointers"
    data-comment-line-37="Also document 'trivial' methods"
    data-comment-line-43="Short description (active speech!)"
    data-comment-line-46="Indicate if nullptr is allowed for pointers"
    data-comment-line-49="Add detailed description and background information if necessary"
    data-comment-line-63="Document side effects and object ownership"
    data-comment-line-66="Provide links to related documentation"
    data-comment-line-78="Indicate value range and/or units of arguments"
    data-comment-line-80="Indicate value range and/or units of arguments"
    data-comment-line-82="Indicate value range and/or units of arguments"
    data-comment-line-93="Indicate data type of arguments (if unclear)"
    data-comment-line-95="Indicate value range and/or units of arguments"
    data-comment-line-101="Indicate origin of overridden functions"
    data-comment-line-106="Document variables directly after their declaration"

{% endcomment %}


{% highlight cpp %}
#pragma once
[...]


namespace gloperate
{


/**
*  @brief
*    Representation of a surface into which can be rendered
*
*    A surface is attached to a window or offscreen context and handles the
*    actual rendering. It should be embedded by the windowing backend and
*    receives state changes from the outside (such as window size, mouse, or
*    keyboard events) and passes them on to the rendering components.
*/
class GLOPERATE_API Surface : public AbstractSurface
{
public:
    cppexpose::Signal<> redrawNeeded;   ///< Called when the surface needs to be redrawn


public:
    /**
    *  @brief
    *    Constructor
    *
    *  @param[in] viewerContext
    *    Viewer context to which the surface belongs (must NOT be null!)
    */
    Surface(ViewerContext * viewerContext);

    /**
    *  @brief
    *    Destructor
    */
    virtual ~Surface();

    /**
    *  @brief
    *    Get OpenGL context
    *
    *  @return
    *    OpenGL context used for rendering on the surface (can be null)
    *
    *  @remarks
    *    The returned context can be null if the surface has not been
    *    initialized yet, or the method is called between onContextDeinit()
    *    and onContextInit() when the context has been changed.
    *    Aside from that, there should always be a valid OpenGL context
    *    attached to the surface.
    */
    AbstractGLContext * openGLContext() const;

    [...]

    /**
    *  @brief
    *    De-Initialize in OpenGL context
    *
    *    This function is called when the OpenGL context is destroyed.
    *    The object must release its OpenGL objects at this point.
    *
    *  @see onContextInit()
    */
    virtual void onContextDeinit();

    /**
    *  @brief
    *    Background color changed
    *
    *    This function is called when the viewer has changed its background,
    *    e.g., because a new theme is being applied.
    *
    *  @param[in] red
    *    Red color component (0..1)
    *  @param[in] green
    *    Green color component (0..1)
    *  @param[in] blue
    *    Blue color component (0..1)
    */
    virtual void onBackgroundColor(float red, float green, float blue);

    /**
    *  @brief
    *    Mouse button pressed
    *
    *    This function is called when a mouse button has been pressed inside the viewer.
    *
    *  @param[in] button
    *    Mouse button (see gloperate::MouseButton)
    *  @param[in] pos
    *    Mouse position (real device coordinates)
    */
    virtual void onMousePress(int button, const glm::ivec2 & pos);


protected:
    // Virtual AbstractSurface interface
    virtual void onInit() override;


protected:
    ViewerContext     * m_viewerContext; ///< Viewer context to which the surface belongs
    AbstractGLContext * m_openGLContext; ///< OpenGL context used for rendering on the surface
};


} // namespace gloperate
{% endhighlight %}
