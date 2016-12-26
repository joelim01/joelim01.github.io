---
layout: post
title:  "ActiveRecord Messages, Senders, and Recipients"
date:   2016-12-26 17:34:18 -0500
---


In my final project I decided to create a messaging app. It falls on the whimsical side of things, I'll admit.  

The point isn't so much to maximize utility but to re-capture some of the experience of sending a physical note or letter in an electronic messaging service. When you mail a message you are left with an anticipated but ultimately unknown delivery date. On the receiving end you're unable to respond immediately, at least in the same medium. Its a mode of communication that, for whatever reason, I associate more with contemplation, crafted messages and long form than, say, email or text.  

In coming up with an appropriate data schema I realized pretty quickly that if a message could have multiple recipients (I decided to bend this restricton of physical messages a little bit) I needed to think about how to set up the associtions for  messages and users. 

This is what I came up with:

```
class User < ApplicationRecord
  include ActiveModel::Serializers::JSON
	...
  has_many :sent_messages, :through => :message_senders, :source => :message
  has_many :received_messages, :through => :message_recipients, :source=> :message
  has_many :message_senders, :foreign_key => 'sender_id'
  has_many :message_recipients, :foreign_key => 'recipient_id'
  ...
end

class Message < ApplicationRecord
  include ActiveModel::Serializers::JSON
  has_one :message_sender, :dependent => :destroy
  has_one :sender, :class_name => "User", through: :message_sender
  has_many :message_recipients, :dependent => :destroy
  has_many :recipients, :class_name => "User", through: :message_recipients
end

class MessageRecipient < ApplicationRecord
  belongs_to :recipient, :class_name=> "User"
  belongs_to :message
end

class MessageSender < ApplicationRecord
  belongs_to :sender, :class_name=> "User"
  belongs_to :message
end

```

The double join table between users (message_sender and message_recipients) allows for the creation of an attribute in the table that remembers whether or not a sender or recipient has deleted the message from their view. This allows for the control of the view without deleting the message (and its content) or the join table entries (which contain important information about the message and its senders/recipients) to preserve the integrity of the information for other users.

```
  create_table "message_recipients", force: :cascade do |t|
    t.integer  "message_id"
    t.integer  "recipient_id"
    t.datetime "received_on"
    t.datetime "created_at",   null: false
    t.datetime "updated_at",   null: false  
		t.boolean  "read"  
    t.boolean  "display"
  end

  create_table "message_senders", force: :cascade do |t|
    t.integer  "message_id"
    t.integer  "sender_id"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false  
		t.boolean  "read"  
		t.boolean  "display"
  end
```

The other thing worth noting here is the use of class_name: in the Message model to allow for Users to be both a sender and a recipient and to simplify the language of queries for instance, Message.find(_id_).sender. 

