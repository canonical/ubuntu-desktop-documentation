
# org.a11y.atspi.Accessible

## Description

Base interface that defines basic information about the object, where in the object hierarchy it is located, relations and additional interfaces.

Is implemented by all accessible objects.

## Properties 

### org.a11y.atspi.Accessible:Name 

    Name readable s

Human-readable, localized, short name for the object. Normally you need
to set this for objects which do not have a labelled-by relation.
Consider a widget to select RGB colors by setting three sliders. The
names for the sliders would be \"Red\", \"Green\", \"Blue\",
respectively, or translations to application\'s locale. The names would
be unnecessary if each slider had a labelled-by relation to
corresponding labels visible in the user interface.

In general, something is missing from your application if an object that
can be interacted with has no Name or a labelled-by relation.

### org.a11y.atspi.Accessible:Description 

    Description readable s

Human-readable, localized description of the object in more detail.
While the Name property is meant to be a short string that screen
readers say during normal navigation, the Description property is for
when the user asks for more detail.

### org.a11y.atspi.Accessible:Parent 

    Parent readable (so)

Accessible parent object of the current object. The (so) is a string for
the application name, and an object path.

Null parent: If the object has no parent (e.g. the application\'s root
object is being queried), return \"\" for the application name name and
\"/org/a11y/atspi/null\" for the object path.

Root object: An application must have a single root object, called
\"/org/a11y/atspi/accessible/root\". All other objects should have that
one as their highest-level ancestor.

### org.a11y.atspi.Accessible:ChildCount 



    ChildCount readable i

number of accessible children for this object.

### org.a11y.atspi.Accessible:Locale 



    Locale readable s



Unix locale for the current object. For an application, this may be the
locale for the language that the application shows in its user
interface.

For a document being shown in an application, or a paragraph within a
document, the locale may refer to that object exclusively. For example,
an application may be showing itself in English (\"en\"), but it may be
used to display a document in Spanish (\"es\"). In the latter case, a
screen reader will want to know that it should switch to Spanish while
reading the document.

### org.a11y.atspi.Accessible:AccessibleId 



    AccessibleId readable s



Application-specific identifier for the current object. This can be used
to give a special id to an object for reliable identification by ATs or
for use in tests, for example, \"my_widget\". Note that there is no way
to directly find an object by its id; ATs may have to recursively get
the children to find a specific id. This is because accessible objects
can be created dynamically, and they do not always correspond to a
static view of an application\'s data.

### org.a11y.atspi.Accessible:HelpText 



    HelpText readable s



Help text for the current object.

## Methods

### org.a11y.atspi.Accessible.GetChildAtIndex 



    GetChildAtIndex (
      IN index i,
      OUT unnamed_arg1 (so)
    )



Queries the Nth accessible child of the current object. It is expected
that this will correspond to the order that the GetChildren method would
return.



The (so) is a string for the application name, and an object
path.

Notes: implementations vary in their behavior when the index is out of
range. GTK4 returns an error, while atk-adaptor returns the null object
path \"/org/a11y/atspi/null\". To keep the type system gods happy, you
should probably return a DBus error in that case.

index

:   0-based index of the child to query.

unnamed_arg1

### org.a11y.atspi.Accessible.GetChildren 



    GetChildren (
      OUT unnamed_arg0 a(so)
    )



Returns a list of the object\'s accessible children.

Each array element (so) is a string for the application name, and an
object path.

An implementor may return an error if the object exposes a large number
of children such that returning a list would be prohibitive.


### org.a11y.atspi.Accessible.GetIndexInParent



    GetIndexInParent (
      OUT unnamed_arg0 i
    )



Returns the 0-based index at which the object would be returned by
calling GetChildren on its parent, or -1 if the object has no containing
parent or on exception.


### org.a11y.atspi.Accessible.GetRelationSet 



    GetRelationSet (
      OUT unnamed_arg0 a(ua(so))
    )



Returns a set of relationships between the current object and others.
Each element in the outermost array contains a number that indicates the
relation type (see below), and an array of references to accessible
objects to which that relationship applies. Each element in the inner
array is a (so) with a string for the application name, and an object
path.

Each relationship between objects (possibly one-to-many or many-to-one)
allows better semantic identification of how objects are associated with
one another. For instance, the ATSPI_RELATION_LABELLED_BY relationship
may be used to identify labelling information that should accompany the
accessible name property when presenting an object\'s content or
identity to the end user. Similarly, ATSPI_RELATION_CONTROLLER_FOR can
be used to further specify the context in which a valuator is useful,
and/or the other UI components which are directly effected by user
interactions with the valuator. Common examples include association of
scrollbars with the viewport or panel which they control.



Relation types - these are the enum values from AtspiRelationType in
atspi-constants.h:

0 - ATSPI_RELATION_NULL: Not a meaningful relationship; clients should not

:   normally encounter this value.

1 - ATSPI_RELATION_LABEL_FOR: Object is a label for one or more other
objects.

2 - ATSPI_RELATION_LABELLED_BY: Object is labelled by one or more other

:   objects.

3 - ATSPI_RELATION_CONTROLLER_FOR: Object is an interactive object which

:   modifies the state, onscreen location, or other attributes of one or
    more target objects.

4 - ATSPI_RELATION_CONTROLLED_BY: Object state, position, etc. is

:   modified/controlled by user interaction with one or more other
    objects. For instance a viewport or scroll pane may be
    ATSPI_RELATION_CONTROLLED_BY scrollbars.

5 - ATSPI_RELATION_MEMBER_OF: Object has a grouping relationship (e.g. \'same

:   group as\') to one or more other objects.

6 - ATSPI_RELATION_TOOLTIP_FOR: Object is a tooltip associated with another

:   object.

7 - ATSPI_RELATION_NODE_CHILD_OF: Object is a child of the target.

8 - ATSPI_RELATION_NODE_PARENT_OF: Object is a parent of the target.

9 - ATSPI_RELATION_EXTENDED: Used to indicate that a relationship exists, but

:   its type is not specified in the enumeration.

10 - ATSPI_RELATION_FLOWS_TO: Object renders content which flows logically to

:   another object. For instance, text in a paragraph may flow to
    another object which is not the \'next sibling\' in the
    accessibility hierarchy.

11 - ATSPI_RELATION_FLOWS_FROM: Reciprocal of ATSPI_RELATION_FLOWS_TO.

12 - ATSPI_RELATION_SUBWINDOW_OF: Object is visually and semantically considered

:   a subwindow of another object, even though it is not the object\'s
    child. Useful when dealing with embedded applications and other
    cases where the widget hierarchy does not map cleanly to the
    onscreen presentation.

13 - ATSPI_RELATION_EMBEDS: Similar to ATSPI_RELATION_SUBWINDOW_OF, but

:   specifically used for cross-process embedding.

14 - ATSPI_RELATION_EMBEDDED_BY: Reciprocal of ATSPI_RELATION_EMBEDS. Used to

:   denote content rendered by embedded renderers that live in a
    separate process space from the embedding context.

15 - ATSPI_RELATION_POPUP_FOR: Denotes that the object is a transient window or

:   frame associated with another onscreen object. Similar to
    ATSPI_TOOLTIP_FOR, but more general. Useful for windows which are
    technically toplevels but which, for one or more reasons, do not
    explicitly cause their associated window to lose \'window focus\'.
    Creation of an ATSPI_ROLE_WINDOW object with the
    ATSPI_RELATION_POPUP_FOR relation usually requires some presentation
    action on the part of assistive technology clients, even though the
    previous toplevel ATSPI_ROLE_FRAME object may still be the active
    window.

16 - ATSPI_RELATION_PARENT_WINDOW_OF: This is the reciprocal relation to

:   ATSPI_RELATION_POPUP_FOR.

17 - ATSPI_RELATION_DESCRIPTION_FOR: Reciprocal of ATSPI_RELATION_DESCRIBED_BY.

:   Indicates that this object provides descriptive information about
    the target object(s). See also ATSPI_RELATION_DETAILS_FOR and
    ATSPI_RELATION_ERROR_FOR.

18 - ATSPI_RELATION_DESCRIBED_BY: Reciprocal of ATSPI_RELATION_DESCRIPTION_FOR.

:   Indicates that one or more target objects provide descriptive
    information about this object. This relation type is most
    appropriate for information that is not essential as its
    presentation may be user-configurable and/or limited to an on-demand
    mechanism such as an assistive technology command. For brief,
    essential information such as can be found in a widget\'s on-screen
    label, use ATSPI_RELATION_LABELLED_BY. For an on-screen error
    message, use ATSPI_RELATION_ERROR_MESSAGE. For lengthy extended
    descriptive information contained in an on-screen object, consider
    using ATSPI_RELATION_DETAILS as assistive technologies may provide a
    means for the user to navigate to objects containing detailed
    descriptions so that their content can be more closely reviewed.

19 - ATSPI_RELATION_DETAILS: Reciprocal of ATSPI_RELATION_DETAILS_FOR. Indicates

:   that this object has a detailed or extended description, the
    contents of which can be found in the target object(s). This
    relation type is most appropriate for information that is
    sufficiently lengthy as to make navigation to the container of that
    information desirable. For less verbose information suitable for
    announcement only, see ATSPI_RELATION_DESCRIBED_BY. If the detailed
    information describes an error condition, ATSPI_RELATION_ERROR_FOR
    should be used instead. Since 2.26.

20 - ATSPI_RELATION_DETAILS_FOR: Reciprocal of ATSPI_RELATION_DETAILS. Indicates

:   that this object provides a detailed or extended description about
    the target object(s). See also ATSPI_RELATION_DESCRIPTION_FOR and
    ATSPI_RELATION_ERROR_FOR. Since 2.26.

21 - ATSPI_RELATION_ERROR_MESSAGE: Reciprocal of ATSPI_RELATION_ERROR_FOR.

:   Indicates that this object has one or more errors, the nature of
    which is described in the contents of the target object(s). Objects
    that have this relation type should also contain
    ATSPI_STATE_INVALID_ENTRY when their GetState method is called.
    Since: 2.26.

22 - ATSPI_RELATION_ERROR_FOR: Reciprocal of ATSPI_RELATION_ERROR_MESSAGE.

:   Indicates that this object contains an error message describing an
    invalid condition in the target object(s). Since: 2.26.


### org.a11y.atspi.Accessible.GetRole 



    GetRole (
      OUT unnamed_arg0 u
    )



Gets the accessible role that the current object represents. Roles make
it possible for various UI toolkits to expose their controls to
assistive technologies (ATs) with a standard interface, regardless of
toolkit. For example, a widget that acts like a conventional push button
(appears unpressed; presses when acted upon; invokes a certain action
when pressed) can expose an ATSPI_ROLE_BUTTON role.



Role values - these are the enum values from AtspiRole in
atspi-constants.h:

0 - ATSPI_ROLE_INVALID: A role indicating an error condition, such as

:   uninitialized Role data.

1 - ATSPI_ROLE_ACCELERATOR_LABEL: Object is a label indicating the keyboard

:   accelerators for the parent.

2 - ATSPI_ROLE_ALERT: Object is used to alert the user about something.

3 - ATSPI_ROLE_ANIMATION: Object contains a dynamic or moving image of some

:   kind.

4 - ATSPI_ROLE_ARROW: Object is a 2D directional indicator.

5 - ATSPI_ROLE_CALENDAR: Object contains one or more dates, usually arranged

:   into a 2D list.

6 - ATSPI_ROLE_CANVAS: Object that can be drawn into and is used to trap

:   events.

7 - ATSPI_ROLE_CHECK_BOX: A choice that can be checked or unchecked and

:   provides a separate indicator for the current state.

8 - ATSPI_ROLE_CHECK_MENU_ITEM: A menu item that behaves like a check box. See

:   ATSPI_ROLE_CHECK_BOX.

9 - ATSPI_ROLE_COLOR_CHOOSER: A specialized dialog that lets the user choose a

:   color.

10 - ATSPI_ROLE_COLUMN_HEADER: The header for a column of data.

11 - ATSPI_ROLE_COMBO_BOX: A list of choices the user can select from.

12 - ATSPI_ROLE_DATE_EDITOR: An object which allows entry of a date.

13 - ATSPI_ROLE_DESKTOP_ICON: An inconified internal frame within a
DESKTOP_PANE.

14 - ATSPI_ROLE_DESKTOP_FRAME: A pane that supports internal frames and

:   iconified versions of those internal frames.

15 - ATSPI_ROLE_DIAL: An object that allows a value to be changed via rotating a

:   visual element, or which displays a value via such a rotating
    element.

16 - ATSPI_ROLE_DIALOG: A top level window with title bar and a border.

17 - ATSPI_ROLE_DIRECTORY_PANE: A pane that allows the user to navigate through

:   and select the contents of a directory.

18 - ATSPI_ROLE_DRAWING_AREA: An object used for drawing custom user interface

:   elements.

19 - ATSPI_ROLE_FILE_CHOOSER: A specialized dialog that displays the files in

:   the directory and lets the user select a file, browse a different
    directory, or specify a filename.

20 - ATSPI_ROLE_FILLER: A object that fills up space in a user
interface.

21 - ATSPI_ROLE_FOCUS_TRAVERSABLE: Don\'t use, reserved for future use.

22 - ATSPI_ROLE_FONT_CHOOSER: Allows selection of a display font.

23 - ATSPI_ROLE_FRAME: A top level window with a title bar, border, menubar,

:   etc.

24 - ATSPI_ROLE_GLASS_PANE: A pane that is guaranteed to be painted on top of

:   all panes beneath it.

25 - ATSPI_ROLE_HTML_CONTAINER: A document container for HTML, whose children

:   represent the document content.

26 - ATSPI_ROLE_ICON: A small fixed size picture, typically used to decorate

:   components.

27 - ATSPI_ROLE_IMAGE: An image, typically static.

28 - ATSPI_ROLE_INTERNAL_FRAME: A frame-like object that is clipped by a desktop

:   pane.

29 - ATSPI_ROLE_LABEL: An object used to present an icon or short string in an

:   interface.

30 - ATSPI_ROLE_LAYERED_PANE: A specialized pane that allows its children to be

:   drawn in layers, providing a form of stacking order.

31 - ATSPI_ROLE_LIST: An object that presents a list of objects to the user and

:   allows the user to select one or more of them.

32 - ATSPI_ROLE_LIST_ITEM: An object that represents an element of a
list.

33 - ATSPI_ROLE_MENU: An object usually found inside a menu bar that contains a

:   list of actions the user can choose from.

34 - ATSPI_ROLE_MENU_BAR: An object usually drawn at the top of the primary

:   dialog box of an application that contains a list of menus the user
    can choose from.

35 - ATSPI_ROLE_MENU_ITEM: An object usually contained in a menu that presents

:   an action the user can choose.

36 - ATSPI_ROLE_OPTION_PANE: A specialized pane whose primary use is inside a

:   dialog.

37 - ATSPI_ROLE_PAGE_TAB: An object that is a child of a page tab list.

38 - ATSPI_ROLE_PAGE_TAB_LIST: An object that presents a series of panels (or

:   page tabs), one at a time,through some mechanism provided by the
    object.

39 - ATSPI_ROLE_PANEL: A generic container that is often used to group
objects.

40 - ATSPI_ROLE_PASSWORD_TEXT: A text object uses for passwords, or other places

:   where the text content is not shown visibly to the user.

41 - ATSPI_ROLE_POPUP_MENU: A temporary window that is usually used to offer the

:   user a list of choices, and then hides when the user selects one of
    those choices.

42 - ATSPI_ROLE_PROGRESS_BAR: An object used to indicate how much of a task has

:   been completed.

43 - ATSPI_ROLE_BUTTON: An object the user can manipulate to tell the

:   application to do something.

44 - ATSPI_ROLE_RADIO_BUTTON: A specialized check box that will cause other

:   radio buttons in the same group to become unchecked when this one is
    checked.

45 - ATSPI_ROLE_RADIO_MENU_ITEM: Object is both a menu item and a \"radio button\".

:   See ATSPI_ROLE_RADIO_BUTTON.

46 - ATSPI_ROLE_ROOT_PANE: A specialized pane that has a glass pane and a

:   layered pane as its children.

47 - ATSPI_ROLE_ROW_HEADER: The header for a row of data.

48 - ATSPI_ROLE_SCROLL_BAR: An object usually used to allow a user to

:   incrementally view a large amount of data by moving the bounds of a
    viewport along a one-dimensional axis.

49 - ATSPI_ROLE_SCROLL_PANE: An object that allows a user to incrementally view

:   a large amount of information. Scroll pane objects are usually
    accompanied by ATSPI_ROLE_SCROLL_BAR controllers, on which the
    ATSPI_RELATION_CONTROLLER_FOR and ATSPI_RELATION_CONTROLLED_BY
    reciprocal relations are set. See the GetRelationSet method.

50 - ATSPI_ROLE_SEPARATOR: An object usually contained in a menu to provide a

:   visible and logical separation of the contents in a menu.

51 - ATSPI_ROLE_SLIDER: An object that allows the user to select from a bounded range.

:   Unlike ATSPI_ROLE_SCROLL_BAR, ATSPI_ROLE_SLIDER objects need not
    control \'viewport\'-like objects.

52 - ATSPI_ROLE_SPIN_BUTTON: An object which allows one of a set of choices to

:   be selected, and which displays the current choice.

53 - ATSPI_ROLE_SPLIT_PANE: A specialized panel that presents two other panels

:   at the same time.

54 - ATSPI_ROLE_STATUS_BAR: Object displays non-quantitative status information

:   (c.f. ATSPI_ROLE_PROGRESS_BAR)

55 - ATSPI_ROLE_TABLE: An object used to represent information in terms of rows

:   and columns.

56 - ATSPI_ROLE_TABLE_CELL: A \'cell\' or discrete child within a Table. Note:

:   Table cells need not have ATSPI_ROLE_TABLE_CELL, other role values
    are valid as well.

57 - ATSPI_ROLE_TABLE_COLUMN_HEADER: An object which labels a particular column

:   in a Table interface interface.

58 - ATSPI_ROLE_TABLE_ROW_HEADER: An object which labels a particular row in a

:   Table interface. Table rows and columns may also be labelled via the
    ATSPI_RELATION_LABEL_FOR/ATSPI_RELATION_LABELLED_BY relationships;
    see the GetRelationSet method.

59 - ATSPI_ROLE_TEAROFF_MENU_ITEM: Object allows menu to be removed from menubar

:   and shown in its own window.

60 - ATSPI_ROLE_TERMINAL: An object that emulates a terminal.

61 - ATSPI_ROLE_TEXT: An interactive widget that supports multiple lines of text

:   and optionally accepts user input, but whose purpose is not to
    solicit user input. Thus ATSPI_ROLE_TEXT is appropriate for the text
    view in a plain text editor but inappropriate for an input field in
    a dialog box or web form. For widgets whose purpose is to solicit
    input from the user, see ATSPI_ROLE_ENTRY and
    ATSPI_ROLE_PASSWORD_TEXT. For generic objects which display a brief
    amount of textual information, see ATSPI_ROLE_STATIC.

62 - ATSPI_ROLE_TOGGLE_BUTTON: A specialized push button that can be checked or

:   unchecked, but does not procide a separate indicator for the current
    state.

63 - ATSPI_ROLE_TOOL_BAR: A bar or palette usually composed of push buttons or

:   toggle buttons.

64 - ATSPI_ROLE_TOOL_TIP: An object that provides information about another

:   object.

65 - ATSPI_ROLE_TREE: An object used to repsent hierarchical information to the

:   user.

66 - ATSPI_ROLE_TREE_TABLE: An object that presents both tabular and

:   hierarchical info to the user.

67 - ATSPI_ROLE_UNKNOWN: The object contains some accessible information,

:   but its role is not known.

68 - ATSPI_ROLE_VIEWPORT: An object usually used in a scroll pane, or to

:   otherwise clip a larger object or content renderer to a specific
    onscreen viewport.

69 - ATSPI_ROLE_WINDOW: A top level window with no title or border.

70 - ATSPI_ROLE_EXTENDED: means that the role for this item is known, but not

:   included in the core enumeration. Deprecated since 2.24.

71 - ATSPI_ROLE_HEADER: An object that serves as a document header.

72 - ATSPI_ROLE_FOOTER: An object that serves as a document footer.

73 - ATSPI_ROLE_PARAGRAPH: An object which is contains a single paragraph of

:   text content. See also ATSPI_ROLE_TEXT.

74 - ATSPI_ROLE_RULER: An object which describes margins and tab stops, etc.

:   for text objects which it controls (should have
    ATSPI_RELATION_CONTROLLER_FOR relation to such).

75 - ATSPI_ROLE_APPLICATION: An object corresponding to the toplevel accessible

:   of an application, which may contain ATSPI_ROLE_FRAME objects or
    other accessible objects. Children of objects with the
    ATSPI_ROLE_DESKTOP_FRAME role are generally ATSPI_ROLE_APPLICATION
    objects.

76 - ATSPI_ROLE_AUTOCOMPLETE: The object is a dialog or list containing items

:   for insertion into an entry widget, for instance a list of words for
    completion of a text entry.

77 - ATSPI_ROLE_EDITBAR: The object is an editable text object in a
toolbar.

78 - ATSPI_ROLE_EMBEDDED: The object is an embedded component container. This

:   role is a \"grouping\" hint that the contained objects share a
    context which is different from the container in which this
    accessible is embedded. In particular, it is used for some kinds of
    document embedding, and for embedding of out-of-process component,
    \"panel applets\", etc.

79 - ATSPI_ROLE_ENTRY: The object is a component whose textual content may be

:   entered or modified by the user, provided ATSPI_STATE_EDITABLE is
    present. A readonly ATSPI_ROLE_ENTRY object (i.e. where
    ATSPI_STATE_EDITABLE is not present) implies a read-only \'text
    field\' in a form, as opposed to a title, label, or caption.

80 - ATSPI_ROLE_CHART: The object is a graphical depiction of quantitative data.

:   It may contain multiple subelements whose attributes and/or
    description may be queried to obtain both the quantitative data and
    information about how the data is being presented. The
    ATSPI_LABELLED_BY relation is particularly important in interpreting
    objects of this type, as is the accessible description property. See
    ATSPI_ROLE_CAPTION.

81 - ATSPI_ROLE_CAPTION: The object contains descriptive information, usually

:   textual, about another user interface element such as a table,
    chart, or image.

82 - ATSPI_ROLE_DOCUMENT_FRAME: The object is a visual frame or container which

:   contains a view of document content. Document frames may occur
    within another Document instance, in which case the second document
    may be said to be embedded in the containing instance. HTML frames
    are often ATSPI_ROLE_DOCUMENT_FRAME: Either this object, or a
    singleton descendant, should implement the org.a11y.atspi.Document
    interface.

83 - ATSPI_ROLE_HEADING: The object serves as a heading for content which

:   follows it in a document. The \'heading level\' of the heading, if
    availabe, may be obtained by querying the object\'s attributes.

84 - ATSPI_ROLE_PAGE: The object is a containing instance which encapsulates a

:   page of information. ATSPI_ROLE_PAGE is used in documents and
    content which support a paginated navigation model.

85 - ATSPI_ROLE_SECTION: The object is a containing instance of document content

:   which constitutes a particular \'logical\' section of the document.
    The type of content within a section, and the nature of the section
    division itself, may be obtained by querying the object\'s
    attributes. Sections may be nested.

86 - ATSPI_ROLE_REDUNDANT_OBJECT: The object is redundant with another object in

:   the hierarchy, and is exposed for purely technical reasons. Objects
    of this role should be ignored by clients, if they are encountered
    at all.

87 - ATSPI_ROLE_FORM: The object is a containing instance of document content

:   which has within it components with which the user can interact in
    order to input information; i.e. the object is a container for
    pushbuttons, comboboxes, text input fields, and other \'GUI\'
    components. ATSPI_ROLE_FORM should not, in general, be used for
    toplevel GUI containers or dialogs, but should be reserved for
    \'GUI\' containers which occur within document content, for instance
    within Web documents, presentations, or text documents. Unlike other
    GUI containers and dialogs which occur inside application instances,
    ATSPI_ROLE_FORM containers\' components are associated with the
    current document, rather than the current foreground application or
    viewer instance.

88 - ATSPI_ROLE_LINK: The object is a hypertext anchor, i.e. a \"link\" in a

:   hypertext document. Such objects are distinct from \'inline\'
    content which may also use the Hypertext/Hyperlink interfaces to
    indicate the range/location within a text object where an inline or
    embedded object lies.

89 - ATSPI_ROLE_INPUT_METHOD_WINDOW: The object is a window or similar viewport

:   which is used to allow composition or input of a \'complex
    character\', in other words it is an \"input method window\".

90 - ATSPI_ROLE_TABLE_ROW: A row in a table.

91 - ATSPI_ROLE_TREE_ITEM: An object that represents an element of a
tree.

92 - ATSPI_ROLE_DOCUMENT_SPREADSHEET: A document frame which contains a

:   spreadsheet.

93 - ATSPI_ROLE_DOCUMENT_PRESENTATION: A document frame which contains a

:   presentation or slide content.

94 - ATSPI_ROLE_DOCUMENT_TEXT: A document frame which contains textual content,

:   such as found in a word processing application.

95 - ATSPI_ROLE_DOCUMENT_WEB: A document frame which contains HTML or other

:   markup suitable for display in a web browser.

96 - ATSPI_ROLE_DOCUMENT_EMAIL: A document frame which contains email content

:   to be displayed or composed either in plain text or HTML.

97 - ATSPI_ROLE_COMMENT: An object found within a document and designed to

:   present a comment, note, or other annotation. In some cases, this
    object might not be visible until activated.

98 - ATSPI_ROLE_LIST_BOX: A non-collapsible list of choices the user can
select from.

99 - ATSPI_ROLE_GROUPING: A group of related widgets. This group
typically has a label.

100 - ATSPI_ROLE_IMAGE_MAP: An image map object. Usually a graphic with multiple

:   hotspots, where each hotspot can be activated resulting in the
    loading of another document or section of a document.

101 - ATSPI_ROLE_NOTIFICATION: A transitory object designed to present a

:   message to the user, typically at the desktop level rather than
    inside a particular application.

102 - ATSPI_ROLE_INFO_BAR: An object designed to present a message to the user

:   within an existing window.

103 - ATSPI_ROLE_LEVEL_BAR: A bar that serves as a level indicator to, for

:   instance, show the strength of a password or the state of a battery.
    Since: 2.8

104 - ATSPI_ROLE_TITLE_BAR: A bar that serves as the title of a window or a

:   dialog. Since: 2.12

105 - ATSPI_ROLE_BLOCK_QUOTE: An object which contains a text section

:   that is quoted from another source. Since: 2.12

106 - ATSPI_ROLE_AUDIO: An object which represents an audio

:   element. Since: 2.12

107 - ATSPI_ROLE_VIDEO: An object which represents a video

:   element. Since: 2.12

108 - ATSPI_ROLE_DEFINITION: A definition of a term or concept. Since:
2.12

109 - ATSPI_ROLE_ARTICLE: A section of a page that consists of a

:   composition that forms an independent part of a document, page, or
    site. Examples: A blog entry, a news story, a forum post. Since:
    2.12

110 - ATSPI_ROLE_LANDMARK: A region of a web page intended as a

:   navigational landmark. This is designed to allow Assistive
    Technologies to provide quick navigation among key regions within a
    document. Since: 2.12

111 - ATSPI_ROLE_LOG: A text widget or container holding log content, such

:   as chat history and error logs. In this role there is a relationship
    between the arrival of new items in the log and the reading order.
    The log contains a meaningful sequence and new information is added
    only to the end of the log, not at arbitrary points. Since: 2.12

112 - ATSPI_ROLE_MARQUEE: A container where non-essential information

:   changes frequently. Common usages of marquee include stock tickers
    and ad banners. The primary difference between a marquee and a log
    is that logs usually have a meaningful order or sequence of
    important content changes. Since: 2.12

113 - ATSPI_ROLE_MATH: A text widget or container that holds a mathematical

:   expression. Since: 2.12

114 - ATSPI_ROLE_RATING: A widget whose purpose is to display a rating,

:   such as the number of stars associated with a song in a media
    player. Objects of this role should also implement the Value
    interface. Since: 2.12

115 - ATSPI_ROLE_TIMER: An object containing a numerical counter which

:   indicates an amount of elapsed time from a start point, or the time
    remaining until an end point. Since: 2.12

116 - ATSPI_ROLE_STATIC: A generic non-container object whose purpose is to display

:   a brief amount of information to the user and whose role is known by
    the implementor but lacks semantic value for the user. Examples in
    which ATSPI_ROLE_STATIC is appropriate include the message displayed
    in a message box and an image used as an alternative means to
    display text. ATSPI_ROLE_STATIC should not be applied to widgets
    which are traditionally interactive, objects which display a
    significant amount of content, or any object which has an accessible
    relation pointing to another object. The displayed information, as a
    general rule, should be exposed through the accessible name of the
    object. For labels which describe another widget, see
    ATSPI_ROLE_LABEL. For text views, see ATSPI_ROLE_TEXT. For generic
    containers, see ATSPI_ROLE_PANEL. For objects whose role is not
    known by the implementor, see ATSPI_ROLE_UNKNOWN. Since: 2.16.

117 - ATSPI_ROLE_MATH_FRACTION: An object that represents a mathematical
fraction. Since: 2.16.

118 - ATSPI_ROLE_MATH_ROOT: An object that represents a mathematical expression

:   displayed with a radical. Since: 2.16.

119 - ATSPI_ROLE_SUBSCRIPT: An object that contains text that is displayed as a

:   subscript. Since: 2.16.

120 - ATSPI_ROLE_SUPERSCRIPT: An object that contains text that is displayed as a

:   superscript. Since: 2.16.

121 - ATSPI_ROLE_DESCRIPTION_LIST: An object that represents a list of term-value

:   groups. A term-value group represents an individual description and
    consist of one or more names (ATSPI_ROLE_DESCRIPTION_TERM) followed
    by one or more values (ATSPI_ROLE_DESCRIPTION_VALUE). For each list,
    there should not be more than one group with the same term name.
    Since: 2.26.

122 - ATSPI_ROLE_DESCRIPTION_TERM: An object that represents a term or phrase

:   with a corresponding definition. Since: 2.26.

123 - ATSPI_ROLE_DESCRIPTION_VALUE: An object that represents the description,

:   definition, or value of a term. Since: 2.26.

124 - ATSPI_ROLE_FOOTNOTE: An object that contains the text of a
footnote. Since: 2.26.

125 - ATSPI_ROLE_CONTENT_DELETION: Content previously deleted or proposed to be

:   deleted, e.g. in revision history or a content view providing
    suggestions from reviewers. Since: 2.34.

126 - ATSPI_ROLE_CONTENT_INSERTION: Content previously inserted or proposed to be

:   inserted, e.g. in revision history or a content view providing
    suggestions from reviewers. Since: 2.34.

127 - ATSPI_ROLE_MARK: A run of content that is marked or highlighted, such as for

:   reference purposes, or to call it out as having a special purpose.
    If the marked content has an associated section in the document
    elaborating on the reason for the mark, then an
    ATSPI_RELATION_DETAILS relation should be used on the mark to point
    to that associated section. In addition, the reciprocal relation
    ATSPI_RELATION_DETAILS_FOR should be used on the associated content
    section to point back to the mark. See the GetRelationSet method.
    Since: 2.36.

128 - ATSPI_ROLE_SUGGESTION: A container for content that is called out as a

:   proposed change from the current version of the document, such as by
    a reviewer of the content. An object with this role should include
    children with ATSPI_ROLE_CONTENT_DELETION and/or
    ATSPI_ROLE_CONTENT_INSERTION, in any order, to indicate what the
    actual change is. Since: 2.36

129 - ATSPI_ROLE_PUSH_BUTTON_MENU: A specialized push button to open a
menu. Since: 2.46

130 - ATSPI_ROLE_SWITCH: A switch that can be toggled on/off. Since:
2.56

### org.a11y.atspi.Accessible.GetRoleName



    GetRoleName (
      OUT unnamed_arg0 s
    )



Gets a UTF-8 string corresponding to the name of the role played by an
object. This method will return useful values for roles that fall
outside the enumeration used in the GetRole method. Implementing this
method is optional, and it may be removed in a future version of the
API. Libatspi will only call id in the event of an unknown role.

### org.a11y.atspi.Accessible.GetLocalizedRoleName



    GetLocalizedRoleName (
      OUT unnamed_arg0 s
    )



Gets a UTF-8 string corresponding to the name of the role played by an
object, translated to the current locale. This method will return useful
values for roles that fall outside the enumeration used in the GetRole
method. Implementing this method is optional, and it may be removed in a
future version of the API. Libatspi will only call id in the event of an
unknown role.

### org.a11y.atspi.Accessible.GetState



    GetState (
      OUT unnamed_arg0 au
    )



Gets the set of states currently held by an object.



Elements in the array are enumeration values from AtspiStateType in
atspi-constants.h:

0 - ATSPI_STATE_INVALID: Indicates an invalid state - probably an error

:   condition.

1 - ATSPI_STATE_ACTIVE: Indicates a window is currently the active window, or

:   an object is the active subelement within a container or table.
    ATSPI_STATE_ACTIVE should not be used for objects which have
    ATSPI_STATE_FOCUSABLE or ATSPI_STATE_SELECTABLE: Those objects
    should use ATSPI_STATE_FOCUSED and ATSPI_STATE_SELECTED
    respectively. ATSPI_STATE_ACTIVE is a means to indicate that an
    object which is not focusable and not selectable is the
    currently-active item within its parent container.

2 - ATSPI_STATE_ARMED: Indicates that the object is armed.

3 - ATSPI_STATE_BUSY: Indicates the current object is busy, i.e. onscreen

:   representation is in the process of changing, or the object is
    temporarily unavailable for interaction due to activity already in
    progress.

4 - ATSPI_STATE_CHECKED: Indicates this object is currently checked.

5 - ATSPI_STATE_COLLAPSED: Indicates this object is collapsed.

6 - ATSPI_STATE_DEFUNCT: Indicates that this object no longer has a valid

:   backing widget (for instance, if its peer object has been
    destroyed).

7 - ATSPI_STATE_EDITABLE: Indicates the user can change the contents of this

:   object.

8 - ATSPI_STATE_ENABLED: Indicates that this object is enabled, i.e. that it

:   currently reflects some application state. Objects that are \"greyed
    out\" may lack this state, and may lack the ATSPI_STATE_SENSITIVE if
    direct user interaction cannot cause them to acquire
    ATSPI_STATE_ENABLED. See ATSPI_STATE_SENSITIVE.

9 - ATSPI_STATE_EXPANDABLE: Indicates this object allows progressive

:   disclosure of its children.

10 - ATSPI_STATE_EXPANDED: Indicates this object is expanded.

11 - ATSPI_STATE_FOCUSABLE: Indicates this object can accept keyboard focus,

:   which means all events resulting from typing on the keyboard will
    normally be passed to it when it has focus.

12 - ATSPI_STATE_FOCUSED: Indicates this object currently has the keyboard

:   focus.

13 - ATSPI_STATE_HAS_TOOLTIP: Indicates that the object has an associated

:   tooltip.

14 - ATSPI_STATE_HORIZONTAL: Indicates the orientation of this object is

:   horizontal.

15 - ATSPI_STATE_ICONIFIED: Indicates this object is minimized and is

:   represented only by an icon.

16 - ATSPI_STATE_MODAL: Indicates something must be done with this object

:   before the user can interact with an object in a different window.

17 - ATSPI_STATE_MULTI_LINE: Indicates this (text) object can contain multiple

:   lines of text.

18 - ATSPI_STATE_MULTISELECTABLE: Indicates this object allows more than one of

:   its children to be selected at the same time, or in the case of text
    objects, that the object supports non-contiguous text selections.

19 - ATSPI_STATE_OPAQUE: Indicates this object paints every pixel within its

:   rectangular region. It also indicates an alpha value of unity, if it
    supports alpha blending.

20 - ATSPI_STATE_PRESSED: Indicates this object is currently pressed.

21 - ATSPI_STATE_RESIZABLE: Indicates the size of this object\'s size is not

:   fixed.

22 - ATSPI_STATE_SELECTABLE: Indicates this object is the child of an object

:   that allows its children to be selected and that this child is one
    of those children that can be selected.

23 - ATSPI_STATE_SELECTED: Indicates this object is the child of an object that

:   allows its children to be selected and that this child is one of
    those children that has been selected.

24 - ATSPI_STATE_SENSITIVE: Indicates this object is sensitive, e.g. to user

:   interaction. ATSPI_STATE_SENSITIVE usually accompanies.
    ATSPI_STATE_ENABLED for user-actionable controls, but may be found
    in the absence of ATSPI_STATE_ENABLED if the current visible state
    of the control is \"disconnected\" from the application state. In
    such cases, direct user interaction can often result in the object
    gaining ATSPI_STATE_SENSITIVE, for instance if a user makes an
    explicit selection using an object whose current state is ambiguous
    or undefined. See ATSPI_STATE_ENABLED, ATSPI_STATE_INDETERMINATE.

25 - ATSPI_STATE_SHOWING: Indicates this object, the object\'s parent, the

:   object\'s parent\'s parent, and so on, are all \'shown\' to the
    end-user, i.e. subject to \"exposure\" if blocking or obscuring
    objects do not interpose between this object and the top of the
    window stack.

26 - ATSPI_STATE_SINGLE_LINE: Indicates this (text) object can contain only a

:   single line of text.

27 - ATSPI_STATE_STALE: Indicates that the information returned for this object

:   may no longer be synchronized with the application state. This can
    occur if the object has ATSPI_STATE_TRANSIENT, and can also occur
    towards the end of the object peer\'s lifecycle.

28 - ATSPI_STATE_TRANSIENT: Indicates this object is transient.

29 - ATSPI_STATE_VERTICAL: Indicates the orientation of this object is vertical;

:   for example this state may appear on such objects as scrollbars,
    text objects (with vertical text flow), separators, etc.

30 - ATSPI_STATE_VISIBLE: Indicates this object is visible, e.g. has been

:   explicitly marked for exposure to the user. ATSPI_STATE_VISIBLE is
    no guarantee that the object is actually unobscured on the screen,
    only that it is \'potentially\' visible, barring obstruction, being
    scrolled or clipped out of the field of view, or having an ancestor
    container that has not yet made visible. A widget is potentially
    onscreen if it has both ATSPI_STATE_VISIBLE and ATSPI_STATE_SHOWING.
    The absence of ATSPI_STATE_VISIBLE and ATSPI_STATE_SHOWING is
    semantically equivalent to saying that an object is \'hidden\'.

31 - ATSPI_STATE_MANAGES_DESCENDANTS: Indicates that \"active-descendant-changed\"

:   event is sent when children become \'active\' (i.e. are selected or
    navigated to onscreen). Used to prevent need to enumerate all
    children in very large containers, like tables. The presence of
    ATSPI_STATE_MANAGES_DESCENDANTS is an indication to the client that
    the children should not, and need not, be enumerated by the client.
    Objects implementing this state are expected to provide relevant
    state notifications to listening clients, for instance notifications
    of visibility changes and activation of their contained child
    objects, without the client having previously requested references
    to those children.

32 - ATSPI_STATE_INDETERMINATE: Indicates that a check box or other boolean

:   indicator is in a state other than checked or not checked. This
    usually means that the boolean value reflected or controlled by the
    object does not apply consistently to the entire current context.
    For example, a checkbox for the \"Bold\" attribute of text may have
    ATSPI_STATE_INDETERMINATE if the currently selected text contains a
    mixture of weight attributes. In many cases interacting with a
    ATSPI_STATE_INDETERMINATE object will cause the context\'s
    corresponding boolean attribute to be homogenized, whereupon the
    object will lose ATSPI_STATE_INDETERMINATE and a corresponding
    state-changed event will be fired.

33 - ATSPI_STATE_REQUIRED: Indicates that user interaction with this object is

:   \'required\' from the user, for instance before completing the
    processing of a form.

34 - ATSPI_STATE_TRUNCATED: Indicates that an object\'s onscreen content

:   is truncated, e.g. a text value in a spreadsheet cell.

35 - ATSPI_STATE_ANIMATED: Indicates this object\'s visual representation is

:   dynamic, not static. This state may be applied to an object during
    an animated \'effect\' and be removed from the object once its
    visual representation becomes static. Some applications, notably
    content viewers, may not be able to detect all kinds of animated
    content. Therefore the absence of this state should not be taken as
    definitive evidence that the object\'s visual representation is
    static; this state is advisory.

36 - ATSPI_STATE_INVALID_ENTRY: This object has indicated an error condition

:   due to failure of input validation. For instance, a form control may
    acquire this state in response to invalid or malformed user input.

37 - ATSPI_STATE_SUPPORTS_AUTOCOMPLETION: This state indicates that the object

:   in question implements some form of typeahead or pre-selection
    behavior whereby entering the first character of one or more
    sub-elements causes those elements to scroll into view or become
    selected. Subsequent character input may narrow the selection
    further as long as one or more sub-elements match the string. This
    state is normally only useful and encountered on objects that
    implement AtspiSelection. In some cases the typeahead behavior may
    result in full or partial completion of the data in the input field,
    in which case these input events may trigger text-changed events
    from the source.

38 - ATSPI_STATE_SELECTABLE_TEXT: This state indicates that the object in

:   question supports text selection. It should only be exposed on
    objects which implement the AtspiText interface, in order to
    distinguish this state from ATSPI_STATE_SELECTABLE, which infers
    that the object in question is a selectable child of an object which
    implements AtspiSelection. While similar, text selection and
    subelement selection are distinct operations.

39 - ATSPI_STATE_IS_DEFAULT: This state indicates that the object in question is

:   the \'default\' interaction object in a dialog, i.e. the one that
    gets activated if the user presses \"Enter\" when the dialog is
    initially posted.

40 - ATSPI_STATE_VISITED: This state indicates that the object (typically a

:   hyperlink) has already been activated or invoked, with the result
    that some backing data has been downloaded or rendered.

41 - ATSPI_STATE_CHECKABLE: Indicates this object has the potential to

:   be checked, such as a checkbox or toggle-able table cell. Since:
    2.12

42 - ATSPI_STATE_HAS_POPUP: Indicates that the object has a popup

:   context menu or sub-level menu which may or may not be showing. This
    means that activation renders conditional content. Note that
    ordinary tooltips are not considered popups in this context. Since:
    2.12

43 - ATSPI_STATE_READ_ONLY: Indicates that an object which is ENABLED and

:   SENSITIVE has a value which can be read, but not modified, by the
    user. Since: 2.16


### org.a11y.atspi.Accessible.GetAttributes 



    GetAttributes (
      OUT unnamed_arg0 a{ss}
    )



Gets a list of name/value pairs of attributes or annotations for this
object. For typographic, textual, or textually-semantic attributes, see
the Text.GetAttributes method.

### org.a11y.atspi.Accessible.GetApplication 



    GetApplication (
      OUT unnamed_arg0 (so)
    )



Returns a string for the application name, and an object path for the
containing application object.


### org.a11y.atspi.Accessible.GetInterfaces 



    GetInterfaces (
      OUT unnamed_arg0 as
    )



Returns an array of accessible interface names supported by the current
object.


