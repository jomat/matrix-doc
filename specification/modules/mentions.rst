.. Copyright 2018 New Vector Ltd.
..
.. Licensed under the Apache License, Version 2.0 (the "License");
.. you may not use this file except in compliance with the License.
.. You may obtain a copy of the License at
..
..     http://www.apache.org/licenses/LICENSE-2.0
..
.. Unless required by applicable law or agreed to in writing, software
.. distributed under the License is distributed on an "AS IS" BASIS,
.. WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
.. See the License for the specific language governing permissions and
.. limitations under the License.

User, room, and group mentions
==============================

.. _module:mentions:

This module allows users to mention other users, rooms, and groups within
a room message. This is achieved by including a `matrix.to URI`_ in the HTML
body of an `m.room.message`_ event. This module does not have any server-specific
behaviour to it.

Mentions apply only to `m.room.message`_ events where the ``msgtype`` is ``m.text``,
``m.emote``, or ``m.notice``. The ``format`` for the event must be ``org.matrix.custom.html``
and therefore requires a ``formatted_body``.

To make a mention, reference the entity being mentioned in the ``formatted_body``
using an anchor, like so::

    {
        "body": "Hello Alice!",
        "msgtype": "m.text",
        "format": "org.matrix.custom.html",
        "formatted_body": "Hello <a href='https://matrix.to/#/@alice:example.org'>Alice</a>!"
    }


Client behaviour
----------------

In addition to using the appropriate ``matrix.to URI`` for the mention,
clients should use the following guidelines when making mentions in events
to be sent:

* When mentioning users, use the user's potentially ambiguous display name for
  the anchor's text. If the user does not have a display name, use the user's
  ID.

* When mentioning rooms, use the canonical alias for the room. If the room
  does not have a canonical alias, prefer one of the aliases listed on the
  room. If no alias can be found, fall back to the room ID. In all cases,
  use the alias/room ID being linked to as the anchor's text.

* When referencing groups, use the group ID as the anchor's text.

The text component of the anchor should be used in the event's ``body`` where
the mention would normally be represented, as shown in the example above.

Clients should display mentions differently from other elements. For example,
this may be done by changing the background color of the mention to indicate
that it is different from a normal link. 

If the current user is mentioned in a message (either by a mention as defined
in this module or by a push rule), the client should show that mention differently
from other mentions, such as by using a red background color to signify to the
user that they were mentioned.

When clicked, the mention should navigate the user to the appropriate room, group,
or user information.


.. _`matrix.to URI`: ../appendices.html#matrix-to-navigation