---
layout: post
title:  "ActiveRecord Messages, Senders, and Recipients"
date:   2016-12-26 22:34:17 +0000
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


