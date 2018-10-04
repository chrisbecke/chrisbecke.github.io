# RIFT API Documents
## Commands
### Command.Achievement.Watch
Watches an achievement, possibly unwatching another item if the player is already watching the maximum item count. Permitted only on unfinished achievements.
 
    Command.Achievement.Watch(achievement, flag)   -- achievement, boolean
#### Parameters:
**achievement** - The achievement to be affected.
**flag** - The new watch status.
### Command.Auction.Analyze
Requests auction statistics from the server. The results will be sent to Event.Auction.Statistics. This command is throttled by the "auctionanalyze" throttle type, proportional to the number of days requested.
 
    Command.Auction.Analyze(itemtype, begin, end)   -- itemtype, number, number
    Command.Auction.Analyze(itemtype, begin, end, callback)   -- itemtype, number, number, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**end** - UNIX timestamp to end analysis at.
**begin** - UNIX timestamp to begin analysis at.
**itemtype** - Item type to analyze.
#### Restrictions:
Permitted only with the 'auction' interaction active.
### Command.Auction.Bid
Bids or buys out an auction.
 
    Command.Auction.Bid(auction, bid)   -- auction, number
    Command.Auction.Bid(auction, bid, quantity)   -- auction, number, number
    Command.Auction.Bid(auction, bid, callback)   -- auction, number, callbackfunction
    Command.Auction.Bid(auction, bid, quantity, callback)   -- auction, number, number, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**bid** - The amount to bid. To place a buyout, simply bid the buyout value.
**auction** - The auction to be targeted.
**quantity** - How many items to purchase. If omitted, defaults to the entire auction quantity.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'auction' interaction active.
### Command.Auction.Cancel
Cancels an auction.
 
    Command.Auction.Cancel(auction)   -- auction
    Command.Auction.Cancel(auction, callback)   -- auction, callbackfunction
#### Parameters:
**auction** - The auction to be targeted.
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'auction' interaction active.
### Command.Auction.Fulfill
Fulfills an existing buy order.
 
    Command.Auction.Fulfill(auction, item, quantity)   -- auction, item, number
    Command.Auction.Fulfill(auction, item, quantity, callback)   -- auction, item, number, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**item** - The ID of the item to be taken.
**auction** - The auction to be targeted.
**quantity** - Stack size to fulfill.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'auction' interaction active.
### Command.Auction.Order
Posts a new buy order.
 
    Command.Auction.Order(itemtype, quantity, time, buyout)   -- itemtype, number, number, number
    Command.Auction.Order(itemtype, quantity, time, buyout, callback)   -- itemtype, number, number, number, callbackfunction
#### Parameters:
**buyout** - The total buyout for the new auction, in silver. Must be evenly divisible by quantity.
**itemtype** - The item type to be ordered.
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**time** - The duration that the order should last, in hours. Valid values are limited to 12, 24, and 48.
**quantity** - The number of items to be ordered.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'auction' interaction active.
### Command.Auction.Post
Posts a new auction.
 
    Command.Auction.Post(item, time, bid, buyout)   -- item, number, number/nil, number/nil
    Command.Auction.Post(item, time, bid, buyout, partial)   -- item, number, number/nil, number/nil, boolean
    Command.Auction.Post(item, time, bid, buyout, callback)   -- item, number, number/nil, number/nil, callbackfunction
    Command.Auction.Post(item, time, bid, buyout, partial, callback)   -- item, number, number/nil, number/nil, boolean, callbackfunction
#### Parameters:
**buyout** - The buyout for the new auction, in silver. nil if no buyout is desired.
**time** - The duration that the auction should last, in hours. Valid values are limited to 12, 24, and 48.
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**item** - The ID of the item to be auctioned.
**partial** - Whether partial buyouts should be permitted. Must be "false" if a buyout is not provided or if a bid is provided that is different from the buyout. If this parameter is omitted, will default to "true" when possible, or "false" otherwise. 
**bid** - The minimum bid for the new auction, in silver. nil if no bid is desired.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'auction' interaction active.
### Command.Auction.Scan
Requests an auction house scan. For type "search", if "index" is omitted then this function will error if the "auctionfullscan" queue is not ready. Type "search" with "index" will throttle on the "auction" command queue. Other types throttle on the "global" command queue.
 
    Command.Auction.Scan(parameters)   -- table
#### Parameters:
**parameters** - Table containing data about the requested scan.
				type: Type of scan to perform. One of "bids", "buy", "mine", "orders", or "search".
				If type is "bids", "mine", or "orders", no other parameters are allowed.
				If type is "search":
					category: Category to search for. Same as the "category" member returned by Inspect.Item.Detail(). Optional.
					index: Numeric item index to start the scan at. If provided, the auction scan will return the first 50 items immediately following this index.
					levelMax: Maximum level the item must require. Optional.
					levelMin: Minimum level the item must require. Optional.
					priceMax: Maximum price of the item. Optional.
					priceMin: Minimum price of the item. Optional.
					rarity: The minimum rarity to search for. Same as the "rarity" member returned by Inspect.Item.Detail(). Optional.
					role: The role that the item must be compatible with. One of "mage", "rogue", "cleric", "warrior", or "primalist". Optional.
					statType: Which stat to filter based on. One of "armor", "block", "critPower", "critSpell", "dexterity", "dodge", "endurance", "health", "hit", "intelligence", "mana", "powerAttack", "powerSpell", "resistAir", "resistDeath", "resistEarth", "resistFire", "resistLife", "resistWater", "strength", or "wisdom". Optional. If provided, statMin and statMax must also be provided.
					statMax: Maximum allowed value for the filter stat. Optional. If provided, statType and statMin must also be provided.
					statMin: Minimum allowed value for the filter stat. Optional. If provided, statType and statMax must also be provided.
					sort: Which column the requested items should be sorted based on. One of "rarity", "name", "level", "time", "seller", "pricePerUnit", "bid", "buyout", "stack". Optional.
					sortOrder: What order to sort in. One of "ascending" or "descending". Optional.
					text: A string to search for in the item name. Optional.
					type: Type of scan to perform. One of "search", "buy", "mine", or "bids". If "mine" or "bids", must be the only member in the table.
				If type is "buy":
					category: Category to search for. Same as the "category" member returned by Inspect.Item.Detail(). Optional.
					text: A string to search for in the item name. Optional.
#### Restrictions:
This function consumes a hardware event to function. Hardware events include Event.UI.Input.Mouse.*.Down, Event.UI.Input.Mouse.*.Up, and Event.UI.Input.Mouse.Wheel.*.
Permitted only with the 'auction' interaction active.
self.hardwareevent=true (boolean)
### Command.Buff.Cancel
Cancels a buff on the player. Not all buffs are cancelable.
 
    Command.Buff.Cancel(buff)   -- buff
#### Parameters:
**buff** - The ID of the buff to cancel.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Buff.Describe
Requests a detailed description for a given buff.
 
    Command.Buff.Describe(unit, buff)   -- unit, buff
#### Parameters:
**buff** - The ID of the buff to describe.
**unit** - The ID of the unit that the buff is on.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Console.Display
Prints a line of text in a console window of your choice. Note that the line length is limited, including HTML tags.
 
    Command.Console.Display(console, suppressPrefix, text, html)   -- console, boolean, string, boolean
#### Parameters:
**suppressPrefix** - Suppress the automatic addon shortname prefix.
**html** - Enables HTML mode. In HTML mode, a limited number of formatting tags are available: &lt;u>, &lt;font color="#rrggbb">, and &lt;a lua="print('This is a lua script.')">.
**console** - The console to display text into. May be "general", "combat", or a console ID.
**text** - The text to be printed.
### Command.CopyToClipboard
Copies text to clipboard
 
    Command.CopyToClipboard(text)   -- string
#### Parameters:
**text** - The new text for the element.
### Command.Cursor
Changes the contents of the cursor. Ability cursors may be set only if the environment is not in secure mode.
 
    Command.Cursor(hold)   -- variant
#### Parameters:
**hold** - The new cursor. Currently accepts ability, item, itemtype, or non-empty non-wildcard slot IDs. Pass nil to clear the cursor.
### Command.Dimension.Layout.Pickup
Pick up the specified dimension item from the world to your inventory.
 
    Command.Dimension.Layout.Pickup(dimensionItem)   -- dimensionitem
#### Parameters:
**dimensionItem** - The ID of the dimension item to operate on.
### Command.Dimension.Layout.Place
Places the item (dimension or inventory) in your current dimension and modifies the details specified by the values parameter.
 
    Command.Dimension.Layout.Place(item, values)   -- item, table
    Command.Dimension.Layout.Place(dimensionItem, values)   -- dimensionitem, table
#### Parameters:
**item** - The inventory item to operate on.
**values** - Table containing data to set on the dimension item. All parameters are optional and will default to current or default values.
				coordX: The X coordinate of the item.
				coordY: The Y coordinate of the item.
				coordZ: The Z coordinate of the item.
				pitch: The pitch rotation of the item.
				roll: The roll rotation of the item.
				scale: The scale of the item.
				yaw: The yaw rotation of the item.
**dimensionItem** - The ID of the dimension item to operate on.
### Command.Dimension.Layout.Select
Selects or deselects the specified dimension item.
 
    Command.Dimension.Layout.Select(dimensionItem, selected)   -- dimensionitem, boolean
#### Parameters:
**selected** - Indicates if the dimension item should be selected or deselected.
**dimensionItem** - The ID of the dimension item to operate on.
### Command.Event.Attach
Attaches an event handler to an event.
 
    Command.Event.Attach(event, handler, label)   -- eventGlobal, function, string
    Command.Event.Attach(event, handler, label, priority)   -- eventGlobal, function, string, number
#### Parameters:
**handler** - A global event handler function. This will be called when the event fires. The first parameter will be the standard global event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**event** - A global event handle, usually pulled out of the "Event." hierarchy.
### Command.Event.Detach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Command.Event.Detach(event, handler)   -- eventGlobal, function/nil
    Command.Event.Detach(event, handler, label)   -- eventGlobal, function/nil, string/nil
    Command.Event.Detach(event, handler, label, priority)   -- eventGlobal, function/nil, string/nil, number/nil
    Command.Event.Detach(event, handler, label, priority, owner)   -- eventGlobal, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**event** - A global event handle, usually pulled out of the "Event." hierarchy.
**handler** - A global event handler function. This will be called when the event fires. The first parameter will be the standard global event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
### Command.Fragment.Recycle
Recycle planar fragments from your inventory.
 
    Command.Fragment.Recycle(item)   -- item
    Command.Fragment.Recycle(items)   -- table
#### Parameters:
**items** - A table containing a list of the items to be taken.
**item** - The ID of the item to be taken.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Guild.Bank.Deposit
Deposits money into the guild bank.
 
    Command.Guild.Bank.Deposit(coin)   -- number
    Command.Guild.Bank.Deposit(coin, callback)   -- number, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**coin** - The string "coin", used as a value to request the player's money.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'guildbank' interaction active.
### Command.Guild.Bank.Purchase
Purchases a new guild bank vault.
 
    Command.Guild.Bank.Purchase()   -- void
    Command.Guild.Bank.Purchase(callback)   -- callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'guildbank' interaction active.
### Command.Guild.Bank.Withdraw
Withdraws money from the guild bank.
 
    Command.Guild.Bank.Withdraw(coin)   -- number
    Command.Guild.Bank.Withdraw(coin, callback)   -- number, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**coin** - The string "coin", used as a value to request the player's money.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'guildbank' interaction active.
### Command.Guild.Log.Request
Requests a guild log event.
 
    Command.Guild.Log.Request()   -- void
    Command.Guild.Log.Request(callback)   -- callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
self.throttleBulk=true (boolean)
### Command.Guild.Motd
Changes the guild's Message of the Day.
 
    Command.Guild.Motd(motd)   -- string
    Command.Guild.Motd(motd, callback)   -- string, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**motd** - The new Message of the Day.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Guild.Roster.Demote
Demotes a guildmember by one rank.
 
    Command.Guild.Roster.Demote(member)   -- string
    Command.Guild.Roster.Demote(member, callback)   -- string, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**member** - The guildmember to target.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Guild.Roster.Kick
Kicks a guildmember from the guild.
 
    Command.Guild.Roster.Kick(member)   -- string
    Command.Guild.Roster.Kick(member, callback)   -- string, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**member** - The guildmember to target.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Guild.Roster.Note
Changes your guild note.
 
    Command.Guild.Roster.Note(note)   -- string
    Command.Guild.Roster.Note(note, callback)   -- string, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**note** - The new note.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Guild.Roster.NoteOfficer
Changes a guildmember's officer note.
 
    Command.Guild.Roster.NoteOfficer(member, note)   -- string, string
    Command.Guild.Roster.NoteOfficer(member, note, callback)   -- string, string, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**member** - The guildmember to target.
**note** - The new note.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Guild.Roster.Promote
Promotes a guildmember by one rank.
 
    Command.Guild.Roster.Promote(member)   -- string
    Command.Guild.Roster.Promote(member, callback)   -- string, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**member** - The guildmember to target.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Guild.Wall.Delete
Deletes an entry from the guild wall.
 
    Command.Guild.Wall.Delete(wall)   -- guildwall
    Command.Guild.Wall.Delete(wall, callback)   -- guildwall, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**wall** - The ID of the target wall post.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Guild.Wall.Post
Posts an entry to the guild wall.
 
    Command.Guild.Wall.Post(post)   -- string
    Command.Guild.Wall.Post(post, callback)   -- string, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**post** - The item to post.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Guild.Wall.Request
Requests a guild wall event.
 
    Command.Guild.Wall.Request()   -- void
    Command.Guild.Wall.Request(callback)   -- callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
self.throttleBulk=true (boolean)
### Command.Item.Destroy
Destroys an item. Be careful: this really does destroy an item. There is no confirmation dialog and the process is irreversible. This cannot destroy items directly out of the guild bank.
 
    Command.Item.Destroy(target)   -- item
#### Parameters:
**target** - The item ID of the item to destroy.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'item' interaction active.
### Command.Item.Mount.Use
Uses a collected mount.
 
    Command.Item.Mount.Use(mount)   -- item
#### Parameters:
**mount** - The item ID of the collected mount to use.
#### Restrictions:
This function consumes a hardware event to function. Hardware events include Event.UI.Input.Mouse.*.Down, Event.UI.Input.Mouse.*.Up, and Event.UI.Input.Mouse.Wheel.*.
self.hardwareevent=true (boolean)
### Command.Item.Move
Moves an item from one location to another. This cannot move items directly between equipment, wardrobe, or guild bank - you'll have to stop off in the inventory first.
 
    Command.Item.Move(source, destination)   -- item, slot
    Command.Item.Move(source, destination)   -- slot, slot
#### Parameters:
**destination** - The location to move the item. May attempt to stack or swap if there is already an item here.
**source** - The item to move. Must be a slot specifier that refers to an actual item.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'item' interaction active.
### Command.Item.Split
Splits a number of items off a stack.
 
    Command.Item.Split(source, stack)   -- item, number
    Command.Item.Split(source, stack)   -- slot, number
#### Parameters:
**source** - The item to split. May be a slot specifier or an item ID.
**stack** - The number of items to move into the new stack. Must be a positive integer.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'item' interaction active.
### Command.Item.Standard.Drag
Behaves in the same way that the addon system does when an inventory icon is dragged.
 
    Command.Item.Standard.Drag(target)   -- slot
    Command.Item.Standard.Drag(target)   -- item
#### Parameters:
**target** - The item or slot to be targeted.
#### Restrictions:
Permitted only with the 'item' interaction active.
### Command.Item.Standard.Drop
Behaves in the same way that the addon system does when an inventory icon is dropped.
 
    Command.Item.Standard.Drop(target)   -- slot
    Command.Item.Standard.Drop(target)   -- item
#### Parameters:
**target** - The item or slot to be targeted.
#### Restrictions:
This function consumes a hardware event to function. Hardware events include Event.UI.Input.Mouse.*.Down, Event.UI.Input.Mouse.*.Up, and Event.UI.Input.Mouse.Wheel.*.
Permitted only with the 'item' interaction active.
self.hardwareevent=true (boolean)
### Command.Item.Standard.Left
Behaves in the same way that the addon system does when an inventory icon is left-clicked.
 
    Command.Item.Standard.Left(target)   -- slot
    Command.Item.Standard.Left(target)   -- item
#### Parameters:
**target** - The item or slot to be targeted.
#### Restrictions:
This function consumes a hardware event to function. Hardware events include Event.UI.Input.Mouse.*.Down, Event.UI.Input.Mouse.*.Up, and Event.UI.Input.Mouse.Wheel.*.
Permitted only with the 'item' interaction active.
self.hardwareevent=true (boolean)
### Command.Item.Standard.Right
Behaves in the same way that the addon system does when an inventory icon is right-clicked.
 
    Command.Item.Standard.Right(target)   -- slot
    Command.Item.Standard.Right(target)   -- item
#### Parameters:
**target** - The item or slot to be targeted.
#### Restrictions:
This function consumes a hardware event to function. Hardware events include Event.UI.Input.Mouse.*.Down, Event.UI.Input.Mouse.*.Up, and Event.UI.Input.Mouse.Wheel.*.
Permitted only with the 'item' interaction active.
self.hardwareevent=true (boolean)
### Command.Mail.Delete
Deletes a mail.
 
    Command.Mail.Delete(mail)   -- mail
    Command.Mail.Delete(mail, callback)   -- mail, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**mail** - The ID of the mail to be targeted.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'mail' interaction active.
### Command.Mail.Open
Opens a mail, retrieving detailed information for it.
 
    Command.Mail.Open(mail)   -- mail
    Command.Mail.Open(mail, callback)   -- mail, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**mail** - The ID of the mail to be targeted.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'mail' interaction active.
### Command.Mail.Pay
Pays the COD fee on a mail.
 
    Command.Mail.Pay(mail)   -- mail
    Command.Mail.Pay(mail, callback)   -- mail, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**mail** - The ID of the mail to be targeted.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'mail' interaction active.
### Command.Mail.Return
Returns a mail to its sender.
 
    Command.Mail.Return(mail)   -- mail
    Command.Mail.Return(mail, callback)   -- mail, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**mail** - The ID of the mail to be targeted.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'mail' interaction active.
### Command.Mail.Send
Sends mail.
 
    Command.Mail.Send(mail)   -- table
    Command.Mail.Send(mail, callback)   -- table, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**mail** - Table containing data about the mail to send.
				to: The name of the player to send mail to. Required.
				subject: The mail's subject. Required.
				body: The mail's body.
				cod: The money required for the recipient to remove attachments. Mutually exclusive with "coin", requires non-empty "attachments".
				coin: The amount of money attached to this message. Mutually exclusive with "cod".
				attachments: A table listing the item IDs of the items you wish to attach. Maximum of 6.
#### Restrictions:
This function consumes a hardware event to function. Hardware events include Event.UI.Input.Mouse.*.Down, Event.UI.Input.Mouse.*.Up, and Event.UI.Input.Mouse.Wheel.*.
Permitted only with the 'mail' interaction active.
self.hardwareevent=true (boolean)
### Command.Mail.Spam
Marks mail as spam.
 
    Command.Mail.Spam(mail)   -- mail
    Command.Mail.Spam(mail, callback)   -- mail, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**mail** - The ID of the mail to be targeted.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'mail' interaction active.
### Command.Mail.Take
Takes an attached item from mail.
 
    Command.Mail.Take(mail, item)   -- mail, item
    Command.Mail.Take(mail, items)   -- mail, table
    Command.Mail.Take(mail, item, callback)   -- mail, item, callbackfunction
    Command.Mail.Take(mail, items, callback)   -- mail, table, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**item** - The ID of the item to be taken.
**items** - A table containing a list of the items to be taken.
**mail** - The ID of the mail to be targeted.
#### Restrictions:
This function is subject to the "global" command queue.
Permitted only with the 'mail' interaction active.
m] has successfully crafted [Carmintium Lariat]!
### Command.Map.Monitor
Controls the state of the map monitor flag. When enabled, mainmap data will be received, updating every sixty seconds. When disabled, mainmap data will be received only when the built-in map window is open.
 
    Command.Map.Monitor(monitor)   -- boolean
#### Parameters:
**monitor** - The new state of the map monitor flag.
### Command.Map.Waypoint.Clear
Clears the player's current waypoint.
 
    Command.Map.Waypoint.Clear()   -- void
### Command.Map.Waypoint.Set
Sets the player's waypoint.
 
    Command.Map.Waypoint.Set(x, z)   -- number, number
#### Parameters:
**z** - The z coordinate of the waypoint.
**x** - The x coordinate of the waypoint.
### Command.Message.Accept
Sets the environment to accept a given type and identifier of message. Parameters can be replaced with "nil" to act as a wildcard.
 
    Command.Message.Accept(type, identifier)   -- string/nil, string/nil
#### Parameters:
**identifier** - The identifier type of the message. Used for the receiver to filter accepted messages via the Command.Message.Accept() function. Must be at least three characters long.
**type** - The type of message. Valid types include "tell", "channel", "guild", "officer", "party", "raid", "say", "yell", "send".
### Command.Message.Broadcast
Broadcast an unreliable addon message to some number of targets. This message will be dropped silently if the targets do not have the available bandwidth to receive it. The callback will respond with failure only if the message's target is invalid. "tell" messages are subject to the same restrictions as Command.Message.Send(). This command is throttled by the "message" throttle type.
 
    Command.Message.Broadcast(type, target, identifier, data)   -- string, string/nil, string, string
    Command.Message.Broadcast(type, target, identifier, data, callback)   -- string, string/nil, string, string, callbackfunction
#### Parameters:
**target** - The target of this message. Required for "channel" or "tell" message types, must be nil otherwise.
**data** - The data to send. This parameter is binary-safe.
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**type** - The type of message to send. Valid types include "tell", "channel", "guild", "officer", "party", "raid", "say", "yell".
**identifier** - The identifier type of the message. Used for the receiver to filter accepted messages via the Command.Message.Accept() function. Must be at least three characters long.
### Command.Message.Reject
Sets the environment to reject a given type and identifier of message. Directly cancels out single calls to Command.Message.Accept.
 
    Command.Message.Reject(type, identifier)   -- string/nil, string/nil
#### Parameters:
**identifier** - The identifier type of the message. Used for the receiver to filter accepted messages via the Command.Message.Accept() function. Must be at least three characters long.
**type** - The type of message. Valid types include "tell", "channel", "guild", "officer", "party", "raid", "say", "yell", "send".
### Command.Message.Send
Send an reliable addon message to a single target.  The callback will respond with success only once the server has accepted the message for processing and queueing. Failure may occur if the target is invalid or if the target's receive queue is full. Messages may be sent only to players that are nearby, in your guild, in your party or raid, or that have sent you a message or tell during this session. This command is throttled by the "message" throttle type.
 
    Command.Message.Send(target, identifier, data, callback)   -- string, string, string, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**target** - The name of the player to send the message to.
**data** - The data to send. This parameter is binary-safe.
**identifier** - The identifier type of the message. Used for the receiver to filter accepted messages via the Command.Message.Accept() function. Must be at least three characters long.
### Command.Minion.Claim
Claims an award from a minion adventure in "finished" mode.
 
    Command.Minion.Claim(adventure)   -- minionadventure
#### Parameters:
**adventure** - The minion adventure to be affected.
#### Restrictions:
This function consumes a hardware event to function. Hardware events include Event.UI.Input.Mouse.*.Down, Event.UI.Input.Mouse.*.Up, and Event.UI.Input.Mouse.Wheel.*.
self.hardwareevent=true (boolean)
### Command.Minion.Hurry
Hurry a minion adventure in "working" mode.
 
    Command.Minion.Hurry(adventure, currency)   -- minionadventure, string
#### Parameters:
**currency** - The currency to use for this command. May be any of "none", "aventurine", or "credit".
**adventure** - The minion adventure to be affected.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Minion.Send
Send a minion on an adventure.
 
    Command.Minion.Send(minion, adventure)   -- minionminion, minionadventure
    Command.Minion.Send(minion, adventure, currency)   -- minionminion, minionadventure, string
#### Parameters:
**currency** - The currency to use for this command. May be any of "none", "aventurine", or "credit".
**adventure** - The minion adventure to be affected.
**minion** - The minion to be affected.
#### Restrictions:
This function consumes a hardware event to function. Hardware events include Event.UI.Input.Mouse.*.Down, Event.UI.Input.Mouse.*.Up, and Event.UI.Input.Mouse.Wheel.*.
self.hardwareevent=true (boolean)
### Command.Minion.Shuffle
Shuffle an adventure for a new option.
 
    Command.Minion.Shuffle(adventure, currency)   -- minionadventure, string
#### Parameters:
**currency** - The currency to use for this command. May be any of "none", "aventurine", or "credit".
**adventure** - The minion adventure to be affected.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Minion.Unlock
Unlock another minion adventure slot.
 
    Command.Minion.Unlock()   -- void
#### Restrictions:
This function consumes a hardware event to function. Hardware events include Event.UI.Input.Mouse.*.Down, Event.UI.Input.Mouse.*.Up, and Event.UI.Input.Mouse.Wheel.*.
self.hardwareevent=true (boolean)
### Command.Quest.Abandon
Abandons the given quest. Permitted only on personal quests.
 
    Command.Quest.Abandon(quest)   -- quest
#### Parameters:
**quest** - The quest to be affected.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Quest.Share
Shares the given quest. Permitted only on personal quests.
 
    Command.Quest.Share(quest)   -- quest
#### Parameters:
**quest** - The quest to be affected.
#### Restrictions:
This function consumes a hardware event to function. Hardware events include Event.UI.Input.Mouse.*.Down, Event.UI.Input.Mouse.*.Up, and Event.UI.Input.Mouse.Wheel.*.
self.hardwareevent=true (boolean)
### Command.Quest.Track
Tracks the current quest, un-tracking any other quest that may be tracked. Pass in "nil" to cancel all tracking. Permitted only on personal quests.
 
    Command.Quest.Track(quest)   -- quest
    Command.Quest.Track(nil)   -- nil
#### Parameters:
**quest** - The quest to be affected.
**nil** - The value "nil".
### Command.Quest.Watch
Watches a quest, possibly unwatching another item if the player is already watching the maximum item count. Permitted only on personal quests.
 
    Command.Quest.Watch(quest, flag)   -- quest, boolean
#### Parameters:
**quest** - The quest to be affected.
**flag** - The new watch status.
### Command.Queue.Handler
Sets the queue handler function. The queue handler function will be called when a Command.* function is called which relies on a queue for throttling and that queue is full. This function will be passed (queue, owner, func, ...). queue: The identifier of the queue that this function call will wait on. owner: The identifier of the addon that made the function call. func: The function call that has been queued. ...: The argument list that must be passed to func.
 
    Command.Queue.Handler(handler)   -- function
#### Parameters:
**handler** - The new queue handler.
### Command.Slash.Register
Registers a new chat slash command, inserts a new event handle into the Event.Slash hierarchy, and returns that handle. If called multiple times with the same slash command, will return the same handle each time.
 
    eventTable = Command.Slash.Register(slashCommand)   -- table <- string
#### Parameters:
**slashCommand** - The name of the slash command to register.
#### Return Values:
**eventTable** - The event table for your slash command. nil if the slash command could not be registered (usually because it conflicts with a built-in slash command.) Will return the same event table if used twice to register the same slash command.
### Command.Social.Friend.Add
Adds a player to your friend list.
 
    Command.Social.Friend.Add(name)   -- string
#### Parameters:
**name** - The name of the friend.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Social.Friend.Note
Changes a friend's personal note.
 
    Command.Social.Friend.Note(name, note)   -- string, string
#### Parameters:
**name** - The name of the friend.
**note** - The new personal note.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Social.Friend.Remove
Removes a player from your friend list.
 
    Command.Social.Friend.Remove(name)   -- string
#### Parameters:
**name** - The name of the friend.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Social.Ignore.Add
Adds an player to your ignore list.
 
    Command.Social.Ignore.Add(name)   -- string
#### Parameters:
**name** - The name of the ignored player.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Social.Ignore.Note
Changes an ignored player's personal note.
 
    Command.Social.Ignore.Note(name, note)   -- string, string
#### Parameters:
**name** - The name of the ignored player.
**note** - The new personal note.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Social.Ignore.Remove
Removes a player from your ignore list.
 
    Command.Social.Ignore.Remove(name)   -- string
#### Parameters:
**name** - The name of the ignored player.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Storage.Clear
Remove an element from a storage segment. If accessing guild storage, you will need write access to that element, as well as the Delete from Guild Addon Storage permission. This command is throttled by the "storage" throttle type.
 
    Command.Storage.Clear(segment, identifier, callback)   -- string, string, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**identifier** - The identifier of the storage bucket. Must be at least three characters long.
**segment** - The storage segment to access. "player" will access the target's per-player storage, "guild" will access the target's guild's per-guild storage. If this function has no target parameter, then the player will be targeted.
### Command.Storage.Get
Gets a specific element from a target's storage. The result will be sent to Event.Storage.Get. This command is throttled by the "storage" throttle type on both the client and server.
 
    Command.Storage.Get(target, segment, identifier, callback)   -- string, string, string, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**target** - The name of the player whose storage should be accessed. Will work only on players in your guild, players in your group, or players that the addon unit system is aware of.
**identifier** - The identifier of the storage bucket. Must be at least three characters long.
**segment** - The storage segment to access. "player" will access the target's per-player storage, "guild" will access the target's guild's per-guild storage. If this function has no target parameter, then the player will be targeted.
### Command.Storage.List
Lists the visible elements in a target's storage. The results will be sent to Event.Storage.List. This command is throttled by the "storage" throttle type.
 
    Command.Storage.List(target, segment, callback)   -- string, string, callbackfunction
#### Parameters:
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**target** - The name of the player whose storage should be accessed. Will work only on players in your guild, players in your group, or players that the addon unit system is aware of.
**segment** - The storage segment to access. "player" will access the target's per-player storage, "guild" will access the target's guild's per-guild storage. If this function has no target parameter, then the player will be targeted.
### Command.Storage.Set
Sets a storage element.  If accessing guild storage, you will need the Write to Guild Addon Storage permission. If replacing an existing element, you will also need write access to that element as well as the Delete from Guild Addon Storage permission. You cannot create an element that you cannot read, nor can you create an element with Officer write permissions if you are not an Officer. This command is throttled by the "storage" throttle type.
 
    Command.Storage.Set(segment, identifier, read, write, data, callback)   -- string, string, string, string, string, callbackfunction
#### Parameters:
**data** - The data to store. This parameter is binary-safe.
**write** - Write permissions for this element. If the segment is "player", this must be "private", as only you may modify your own storage. If the segment is "guild", this may be "guild" or "officer", where guild is changeable by guildmembers with the Delete from Guild Addon Storage permission, and officer is changeable by guildmembers with the Delete from Guild Addon Storage permission and the Officer Chat permission. The write permission must be "officer" if the read permission is "officer".
**callback** - A standard command callback, used for detecting success or failure. See the "callbackfunction" documentation for more details.
**read** - Read permissions for this element. If the segment is "player", this may be "public", "guild", or "private", where public is readable by anyone, guild is readable by your guildmembers only, and private is readable by the player only. If the segment is "guild", this may be "public", "guild", or "officer", where public is readable by anyone, guild is readable by guildmembers with the Listen permission, and officer is readable by guildmembers with the Officer Chat permission.
**identifier** - The identifier of the storage bucket. Must be at least three characters long.
**segment** - The storage segment to access. "player" will access the target's per-player storage, "guild" will access the target's guild's per-guild storage. If this function has no target parameter, then the player will be targeted.
### Command.System.Flash
Controls the flashing of the Rift taskbar icon.
 
    Command.System.Flash(flash)   -- boolean
#### Parameters:
**flash** - Whether the taskbar icon should flash or not.
### Command.System.Strict
Puts the environment into Strict mode. In Strict mode, deprecated functions and events will throw errors instead of functioning normally.
 
    Command.System.Strict()   -- void
### Command.System.Texture.Record
Begin recording Rift UI textures. The texture list will be transmitted via Event.System.Texture during the logout sequence. Note that this flag persists through character selection and cannot be disabled without restarting the client.
 
    Command.System.Texture.Record()   -- void
### Command.System.Watchdog.Quiet
Significantly increases the time required for the watchdog to generate warnings. Intended for bootup processes or similar long startup sequences that will not adversely affect the player experience. This function may not be called in combat.
 
    Command.System.Watchdog.Quiet()   -- void
### Command.Title.Prefix
Changes the player's prefix title.
 
    Command.Title.Prefix(title)   -- title
    Command.Title.Prefix(title)   -- nil
#### Parameters:
**title** - The ID of the new title to set, or "nil" for no title.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Title.Suffix
Changes the player's suffix title.
 
    Command.Title.Suffix(title)   -- title
    Command.Title.Suffix(title)   -- nil
#### Parameters:
**title** - The ID of the new title to set, or "nil" for no title.
#### Restrictions:
This function is subject to the "global" command queue.
### Command.Tooltip
Changes the displayed tooltip.
 
    Command.Tooltip(target)   -- variant
    Command.Tooltip(target, nil)   -- variant, nil
    Command.Tooltip(owner, buff)   -- unit, buff
#### Parameters:
**buff** - The ID of a buff for the new tooltip.
**nil** - Optional placeholder nil.
**target** - The new tooltip. Currently accepts ability, item, itemtype, or unit IDs. Pass nil to clear the tooltip.
**owner** - The ID of the owner of the buff.
### Command.UI.Error
Display the addon error dialog.
 
    Command.UI.Error(id)   -- error
#### Parameters:
**id** - The ID of the error to be displayed.
### Command.Unit.Menu
Creates a standard context menu targeted at the unit of choice.
 
    Command.Unit.Menu(target)   -- unit
#### Parameters:
**target** - The unit to generate a context menu for.
## Inspectors
### Inspect.Ability.New.Detail
Provides detailed information about abilities.
 
    detail = Inspect.Ability.New.Detail(ability)   -- table <- ability
    details = Inspect.Ability.New.Detail(abilities)   -- table <- table
#### Parameters:
**ability** - The identifier of the ability to retrieve detail for.
**abilities** - A table of identifiers of abilities to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single ability.
**details** - Detail tables for all requested abilities. The key is the ability ID, the value is the ability's detail table.
#### Returned Members:
**rangeMin** - The minimum range of the ability.
**costPlanarCharge** - The amount of planar charges this ability consumes on use.
**focusMin** - The minimum focus to use this ability.
**description** - Description for the ability.
**racial** - Signals that the ability is a racial ability.
**outOfRange** - Signals that the ability is out of range.
**focusMax** - The maximum focus to use this ability.
**rangeMax** - The maximum range of the ability.
**unusable** - Signals that this ability is unusable.
**cooldown** - Cooldown of the ability, in seconds.
**stealthRequired** - Signals that the ability requires the user to be in stealth.
**costCharge** - The amount of charge this ability consumes on use.
**gainCharge** - Amount of charge gained by using the ability.
**id** - The ID of the requested element.
**costSpirit** - The amount of spirit this ability consumes on use.
**currentCooldownBegin** - The time the current cooldown started, in the context of Inspect.Time.Frame.
**currentCooldownPaused** - Indicates that this ability's cooldown is paused.
**target** - The Unit ID of the unit that this ability will be used on if triggered at this moment.
**currentCooldownExpired** - Number of seconds the current cooldown is past its expiration time. Generally indicates lag.
**autoattack** - Autoattack mode of the ability.
**continuous** - Signals that the ability is continuous.
**weapon** - The required equipped weapon for this ability. May be "any", "melee", or "ranged".
**positioned** - Signals that the ability's effect is manually positioned by the user.
**costPower** - The amount of power this ability consumes on use.
**icon** - Resource filename of the ability's icon.
**currentCooldownDuration** - Duration of the current cooldown the ability is influenced by, in seconds.
**castingTime** - Casting time of the ability, in seconds.
**costMana** - The amount of mana this ability consumes on use.
**costEnergy** - The amount of energy this ability consumes on use.
**passive** - Signals that the ability is passive.
**currentCooldownRemaining** - Time remaining in the ability's current cooldown, in seconds.
**idNew** - The new ability ID.
**name** - Name of the ability.
**channeled** - Signals that the ability is channeled.
### Inspect.Ability.New.List
List available abilities.
 
    abilities = Inspect.Ability.New.List()   -- table <- void
#### Return Values:
**abilities** - A table of IDs of the available abilities.
### Inspect.Achievement.Category.Detail
Returns information about achievement categories.
 
    detail = Inspect.Achievement.Category.Detail(category)   -- table <- achievementcategory
    details = Inspect.Achievement.Category.Detail(categories)   -- table <- table
#### Parameters:
**category** - The identifier of the achievement category to retrieve detail for.
**categories** - A table of identifiers of achievement categories to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single achievement category.
**details** - Detail tables for all requested achievement categories. The key is the category ID, the value is the category's detail table.
#### Returned Members:
**parent** - The category's parent, if it has one.
**name** - The name of the achievement category.
**id** - The ID of the requested element.
### Inspect.Achievement.Category.List
Returns a table of valid achievement categories.
 
    categories = Inspect.Achievement.Category.List()   -- table <- void
#### Return Values:
**categories** - Valid achievement categories, in {id = true} format.
### Inspect.Achievement.Detail
Provides detailed information about achievements.
 
    detail = Inspect.Achievement.Detail(achievement)   -- table <- achievement
    details = Inspect.Achievement.Detail(achievements)   -- table <- table
#### Parameters:
**achievement** - The identifier of the achievement to retrieve detail for.
**achievements** - A table of identifiers of achievements to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single achievement.
**details** - Detail tables for all requested achievements. The key is the achievement ID, the value is the achievement's detail table.
#### Returned Members:
**sort** - A number indicating the order that this achievement should be sorted in.
**alliance** - The alliance that this achievement requires, either "guardian", "defiant", or nil.
**icon** - Internal name of the achievement's icon.
**id** - The ID of the requested element.
**category** - ID of the achievement's category.
**watch** - Signals that this achievement is being watched.
**title** - The ID of the title this achievement awards.
**previous** - The ID of the achievement immediately previous to this in a chain.
**score** - The number of points this achievement awards for completion.
**complete** - true is the achievement is completed by this character. If the achievement has been completed by another of the player's characters, gives that character's name.
**requirement** - Table listing the requirements for this achievement. Each item may include multiple members.
				type: The type of the requirement. Valid values include "achievement", "artifactset", "discover", "event", "quest", and "tradeskill".
				name: The name of the requirement.
				count: The count required for completion.
				countDone: The count already completed.
				complete: Signals that this requirement is complete.
				id: The id of whatever this requires.
**name** - The achievement's name.
**description** - The achievement's description.
### Inspect.Achievement.List
Returns a table of all known achievements.
 
    achievements = Inspect.Achievement.List()   -- table <- void
#### Return Values:
**achievements** - All known achievements, in {id = true} format.
### Inspect.Addon.Cpu
Returns recent CPU usage information. This is calculated using an exponential-falloff method.
 
    data = Inspect.Addon.Cpu()   -- table <- void
#### Return Values:
**data** - Recent CPU usage. This takes the format { AddonIdentifier = { SubIdentifier = cpu_used_as_a_fraction_of_one } }. SubIdentifiers are generated by Rift and the format may change without notice.
### Inspect.Addon.Current
Returns the current addon. This information is used internally for counting CPU usage and determining frame ownership.
 
    addonIdentifier = Inspect.Addon.Current()   -- string <- void
#### Return Values:
**addonIdentifier** - The addon's identifier, as written in its TOC file.
### Inspect.Addon.Detail
Provides detailed information about loaded addons.
 
    detail = Inspect.Addon.Detail(addon)   -- table <- string
    details = Inspect.Addon.Detail(addons)   -- table <- table
#### Parameters:
**addon** - An addon identifier.
**addons** - A table containing addon identifiers.
#### Return Values:
**detail** - Detail table for a single addon.
**details** - Detail tables for all requested addons. The key is the addon identifier, the value is the addon's detail table.
#### Returned Members:
**nameShort** - The chosen localization of the TOC NameShort field.
**identifier** - The addon's identifier.
**id** - The ID of the requested element.
**data** - The "data" table provided to the addon at load time.
**toc** - The addon's RiftAddon.toc file, parsed and in table form.
**name** - The chosen localization of the TOC Name field.
**description** - The chosen localization of the TOC Description field.
### Inspect.Addon.List
Lists all the addons that the client has loaded.
 
    addons = Inspect.Addon.List()   -- table <- void
#### Return Values:
**addons** - Map of addon identifier to addon version, or "true" if no version is provided.
### Inspect.Attunement.Progress
Returns information on your current attunement progress.
 
    result = Inspect.Attunement.Progress()   -- table <- void
#### Return Values:
**result** - Table containing information on attunement progress.
#### Returned Members:
**accumulated** - Attunement experience accumulated so far in this attunement level.
**needed** - Quantity of attunement experience needed to gain a level.
**spent** - Number of spent attunement ranks.
**available** - Number of unused attunement ranks.
**rested** - Quantity of available attunement rested experience.
### Inspect.Auction.Detail
Provides detailed information about auctions.
 
    detail = Inspect.Auction.Detail(auction)   -- table <- auction
    details = Inspect.Auction.Detail(auctions)   -- table <- table
#### Parameters:
**auction** - An identifier for the auction to retrieve detail for.
**auctions** - A table containing auction identifiers to retrieve details for. The value is the sort order.
#### Return Values:
**detail** - Detail table for a single auction.
**details** - Detail tables for all requested auctions. The key is the auction ID, the value is the auction's detail table.
#### Returned Members:
**buyout** - The buyout amount for this auction, in silver.
**id** - The ID of the requested element.
**itemStack** - The size of this item stack.
**bid** - The current bid on this auction, in silver.
**remaining** - Time remaining in the auction, as a number of seconds.
**seller** - The name of the auction's seller.
**bidder** - Current high bidder on this auction. Available only if your current character is the bidder or the seller.
**item** - ID of the item involved in the auction.
**itemType** - The item's type specifier.
**partial** - Whether partial buyouts are permitted.
#### Restrictions:
Permitted only with the 'auction' interaction active.
### Inspect.Buff.Detail
Provides detailed information about the buffs on a unit.
 
    detail = Inspect.Buff.Detail(unit, buff)   -- table <- unit, buff
    details = Inspect.Buff.Detail(unit, buffs)   -- table <- unit, table
#### Parameters:
**buff** - An identifier for the buff to retrieve detail for.
**buffs** - A table containing buff identifiers to retrieve details for.
**unit** - The unit to inspect.
#### Return Values:
**detail** - Detail table for a single buff.
**details** - Detail tables for all requested buffs. The key is the buff ID, the value is the buff's detail table.
#### Returned Members:
**id** - The ID of the requested element.
**debuff** - Signals that the buff is a debuff. If this is missing, then it's an actual buff.
**rune** - If this buff is created by a rune, the ID of the rune causing it.
**poison** - Signals that the buff is a poison.
**ability** - The ID of the ability that created this buff. Not guaranteed to exist.
**expired** - Number of seconds the buff is past its expiration time. Generally indicates lag.
**disease** - Signals that the buff is a disease.
**caster** - Unit ID of the buff's caster.
**stack** - Number of stacks on the buff.
**descriptionComplete** - Signals that the buff description is complete.
**noncancelable** - Signals that the buff cannot be voluntarily canceled. Does not show up for debuffs.
**curse** - Signals that the buff is a curse.
**remaining** - Time remaining on the buff, in seconds.
**begin** - The time the buff started, in the context of Inspect.Time.Frame.
**description** - Description for the buff. Numbers may not be accurate - see Command.Buff.Describe().
**type** - The buff type ID.
**duration** - Duration of the buff in seconds.
**name** - Name of the buff.
**icon** - Resource filename of the buff's icon.
### Inspect.Buff.List
List buffs on a unit.
 
    buffs = Inspect.Buff.List(unit)   -- table <- unit
#### Parameters:
**unit** - The unit to inspect.
#### Return Values:
**buffs** - A table of the IDs of the buffs on the unit.
### Inspect.Console.Detail
Provides detailed information about consoles.
 
    detail = Inspect.Console.Detail(console)   -- table <- console
    details = Inspect.Console.Detail(consoles)   -- table <- table
#### Parameters:
**consoles** - A table of consoles to retrieve detail for.
**console** - The identifier of the console to retrieve information for. May also be "general" or "combat".
#### Return Values:
**detail** - Detail table for a single console.
**details** - Detail tables for all requested consoles.
#### Returned Members:
**combatBad** - Indicates that this console displays Combat Bad messages.
**raid** - Indicates that this console displays Raid messages.
**id** - The ID of the requested element.
**craftingSelf** - Indicates that this console displays Crafting Self messages.
**experience** - Indicates that this console displays Experience messages.
**combatOther** - Indicates that this console displays Combat Other messages.
**achievement** - Indicates that this console displays Achievement messages.
**channel** - May contain a table of public channels that this console will display. nil if the console does not display any public channels.
**lootMoney** - Indicates that this console displays Money Loot messages.
**system** - Indicates that this console displays System messages.
**lootGroup** - Indicates that this console displays Group Loot messages.
**notoriety** - Indicates that this console displays Notoriety messages.
**party** - Indicates that this console displays Party messages.
**yell** - Indicates that this console displays Yell messages.
**say** - Indicates that this console displays Say messages.
**lootItem** - Indicates that this console displays Item Loot messages.
**tell** - Indicates that this console displays Tell messages.
**skill** - Indicates that this console displays Skill messages.
**raidWarning** - Indicates that this console displays Raid Warning messages.
**combatNeutral** - Indicates that this console displays Combat Neutral messages.
**emote** - Indicates that this console displays Emote messages.
**guild** - Indicates that this console displays Guild messages.
**name** - The player-chosen name of the console.
**combatGood** - Indicates that this console displays Combat Good messages.
**npc** - Indicates that this console displays NPC messages.
**purchase** - Indicates that this console displays Purchase messages.
**combatSpecial** - Indicates that this console displays Combat Special messages.
**officer** - Indicates that this console displays Officer messages.
**accolade** - Indicates that this console displays Accolade messages.
**craftingOther** - Indicates that this console displays Crafting Others messages.
### Inspect.Console.List
Returns a table of the current open text consoles.
 
    consoles = Inspect.Console.List()   -- table <- void
#### Return Values:
**consoles** - A table containing the IDs of the available consoles.
### Inspect.Currency.Category.Detail
Returns information about currency categories.
 
    detail = Inspect.Currency.Category.Detail(category)   -- table <- currencycategory
    details = Inspect.Currency.Category.Detail(categories)   -- table <- table
#### Parameters:
**category** - The identifier of the currency category to retrieve detail for.
**categories** - A table of identifiers of currency categories to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single currency category.
**details** - Detail tables for all requested currency categories. The key is the category ID, the value is the category's detail table.
#### Returned Members:
**name** - The name of the currency category.
**id** - The ID of the requested element.
m] has successfully crafted [Carmintium Lariat]!
### Inspect.Currency.Category.List
Returns a table of valid currency categories.
 
    categories = Inspect.Currency.Category.List()   -- table <- void
#### Return Values:
**categories** - Valid currency categories, in {id = true} format.
### Inspect.Currency.Detail
Provides detailed information about currencies.
 
    detail = Inspect.Currency.Detail(coin)   -- table <- string
    detail = Inspect.Currency.Detail(currency)   -- table <- itemtype
    details = Inspect.Currency.Detail(currencies)   -- table <- table
#### Parameters:
**coin** - The string "coin", used as a value to request the player's money.
**currencies** - A table of identifiers of currencies to retrieve detail for.
**currency** - The identifier of the currency to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single currency.
**details** - Detail tables for all requested currencies. The key is the currency ID, the value is the currency's detail table.
#### Returned Members:
**icon** - Internal name of the currency's icon.
**id** - The ID of the requested element.
**category** - ID of the currency's category.
**stackMax** - The maximum number of this currency that you can have.
**name** - The currency's name.
**stack** - The number of this currency that you have.
### Inspect.Currency.List
Returns a table of all known currencies.
 
    currencies = Inspect.Currency.List()   -- table <- void
#### Return Values:
**currencies** - All known currencies, in {id = amount} format.
### Inspect.Cursor
Returns the current contents of the cursor.
 
    type, held = Inspect.Cursor()   -- nil, nil <- void
    type, held = Inspect.Cursor()   -- string, variant <- void
#### Return Values:
**held** - The blob describing what is currently held. Generally, some kind of identifier used in another part of the addon system.
**type** - The current cursor type. Valid values include "ability", "item", "itemtype", and nil.
### Inspect.Dimension.Layout.Detail
Returns details about the specified dimension item.
 
    detail = Inspect.Dimension.Layout.Details(dimensionitem)   -- table <- dimensionitem
    details = Inspect.Dimension.Layout.Details(dimensionitems)   -- table <- table
#### Parameters:
**dimensionitem** - The identifier of the dimension item to retrieve detail for. 
**dimensionitems** - A table of identifiers of dimension items to retrieve detail for.
#### Return Values:
**detail** - Table of details about the dimension item specified.
**details** - Detail table for all requsted dimension items. The key is the dimension item ID, the value is the dimension item's detail table.
#### Returned Members:
**scale** - The scale of the item.
**pitch** - The pitch of the item.
**id** - The item's id.
**crated** - Indicates that the item is crated.
**yaw** - The yaw of the item.
**coordY** - The Y coordinate of the item.
**icon** - Resource filename of the item's icon.
**roll** - The roll of the item.
**selected** - Indicates that the item is selected.
**type** - The type of the item.
**coordX** - The X coordinate of the item.
**name** - The name of the item.
**coordZ** - The Z coordinate of the item.
### Inspect.Dimension.Layout.List
Returns a list of dimension items in the dimension you're currently in, and whether or not they are crated.
 
    list = Inspect.Dimension.Layout.List()   -- table <- void
#### Return Values:
**list** - The list of detail results.
### Inspect.Documentation
Provide documentation on items in the addon environment. Called with no parameters, it returns a table listing all documentation. Can provide both human-readable and computer-readable documentation.
 
    documentables = Inspect.Documentation()   -- table <- void
    documentation = Inspect.Documentation(item)   -- string <- variant
    documentation = Inspect.Documentation(item, parseable)   -- string <- variant, boolean
    documentationTable = Inspect.Documentation(item, parseable)   -- table <- variant, boolean
#### Parameters:
**parseable** - Whether to return in a computer-readable format, as opposed to the normal human-readable format.
**item** - The item to get documentation on. May be either the item itself or a string identifier.
#### Return Values:
**documentables** - List of all items that documentation can be retrieved for. In {["itemname"] = true} format.
**documentationTable** - Computer-readable documentation for the requested item. Format may change without warning.
**documentation** - Documentation for the requested item.
#### Returned Members:
**(other)** - May include many other parameters. These parameters are not yet documented.
**readable** - Human-readable string version of this documentation.
**disabled** - Boolean that indicates whether this function is disabled for deprecation reasons.
### Inspect.Event.List
Lists the current event handlers for an event.
 
    result = Inspect.Event.List(event)   -- table <- eventGlobal
#### Parameters:
**event** - A global event handle, usually pulled out of the "Event." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**handler** - The handler that will be called when the event fires.
**priority** - Priority for the event handler.
**label** - Label assigned to the event handler at creation.
**owner** - Owner addon for the event handler.
### Inspect.Experience
Returns information on your experience.
 
    result = Inspect.Experience()   -- table <- void
#### Return Values:
**result** - Table containing information on experience.
#### Returned Members:
**needed** - Quantity of experience needed to gain a level.
**accumulated** - Experience accumulated so far in this character level.
**rested** - Quantity of available rested experience.
### Inspect.Faction.Detail
Provides detailed information about factions.
 
    detail = Inspect.Faction.Detail(faction)   -- table <- faction
    details = Inspect.Faction.Detail(factions)   -- table <- table
#### Parameters:
**factions** - A table of identifiers of factions to retrieve detail for.
**faction** - The identifier of the faction to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single faction.
**details** - Detail tables for all requested factions. The key is the faction ID, the value is the faction's detail table.
#### Returned Members:
**categoryName** - Name of the faction's category.
**notoriety** - The current notoriety you have with this faction.
**name** - The faction's name.
**id** - The ID of the requested element.
### Inspect.Faction.List
Returns a table of all known factions.
 
    factions = Inspect.Faction.List()   -- table <- void
#### Return Values:
**factions** - All known factions, in {id = notoriety} format.
### Inspect.Guild.Bank.Coin
Returns the amount of money contained within the guild bank.
 
    coin = Inspect.Guild.Bank.Coin()   -- number <- void
#### Return Values:
**coin** - The current money in the guild bank, in silver.
#### Restrictions:
Permitted only with the 'guildbank' interaction active.
### Inspect.Guild.Bank.Detail
Provides detailed information about guild bank vaults.
 
    detail = Inspect.Guild.Bank.Detail(guildvault)   -- table <- slot
    details = Inspect.Guild.Bank.Detail(guildvaults)   -- table <- table
#### Parameters:
**guildvaults** - A table of slot specifiers, each representing an entire guild bank vault, to retrieve information for.
**guildvault** - A slot specifier representing the entire guild bank vault to retrieve information for.
#### Return Values:
**detail** - Detail table for a guild bank vault.
**details** - Detail tables for all requested guild bank vault. The key is the vault specifier, the value is the vault's detail table.
#### Returned Members:
**name** - The name of the vault.
**id** - The ID of the requested element.
### Inspect.Guild.Bank.List
Returns a table of all available guild bank vaults.
 
    list = Inspect.Guild.Bank.List()   -- table <- void
#### Return Values:
**list** - All available guild vaults, in {id = name} format.
### Inspect.Guild.Motd
Returns the current guild Message of the Day.
 
    motd = Inspect.Guild.Motd()   -- string <- void
#### Return Values:
**motd** - The current Message of the Day.
### Inspect.Guild.Rank.Detail
Provides detailed information about guild ranks.
 
    detail = Inspect.Guild.Rank.Detail(guildrank)   -- table <- guildrank
    details = Inspect.Guild.Rank.Detail(guildranks)   -- table <- table
#### Parameters:
**guildrank** - The identifier of the rank to retrieve detail for.
**guildranks** - A table of identifiers of ranks to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single rank.
**details** - Detail table for all requested ranks. The key is the rank ID, the value is the rank's detail table.
#### Returned Members:
**noteOfficer** - Signals that this rank has permission to view and edit Officer Notes.
**invite** - Signals that this rank has permission to invite new guildmembers.
**demote** - Signals that this rank has permission to demote guildmembers.
**questAccept** - Signals that this rank has permission to accept guild quests.
**rally** - Signals that this rank has permission to set the guild rally point.
**questAbandon** - Signals that this rank has permission to abandon guild quests.
**addonStorageWrite** - Signals that this rank has permission to non-destructively write addon storage elements.
**listen** - Signals that this rank has permission to listen to guild chat.
**wallPost** - Signals that this rank has permission to post to the Guild Wall.
**wallDelete** - Signals that this rank has permission to delete entries off the Guild Wall.
**finderReceive** - Signals that this rank has permission to receive Guild Finder messages.
**id** - The ID of the requested element.
**finderEdit** - Signals that this rank has permission to edit the Guild Finder settings.
**perk** - Signals that this rank has permission to choose guild perks.
**rename** - Signals that this rank has permission to rename the guild.
**officer** - Signals that this rank has permission to listen to and speak in officer chat.
**speak** - Signals that this rank has permission to speak in guild chat.
**vaultRename** - Signals that this rank has permission to rename guild vaults.
**questComplete** - Signals that this rank has permission to complete guild quests.
**vaultAccess** - A table containing information on vault access permissions. The key is the vault ID in slot format. The value is a table containing two members, "access" which may be nil, "deposit", or "full", and "withdrawLimit" which gives the number of stacks that may be withdrawn daily.
**vaultBuy** - Signals that this rank has permission to buy guild vaults.
**kick** - Signals that this rank has permission to kick guildmembers.
**addonStorageDelete** - Signals that this rank has permission to modify or delete addon storage elements.
**rankEdit** - Signals that this rank has permission to edit rank permissions.
**motd** - Signals that this rank has permission to change the guild Message of the Day.
**name** - The name of this rank.
**promote** - Signals that this rank has permission to promote guildmembers.
### Inspect.Guild.Rank.List
Lists available guild ranks.
 
    list = Inspect.Guild.Rank.List()   -- table <- void
#### Return Values:
**list** - A table of the IDs of the available ranks. The value is the rank's name.
### Inspect.Guild.Roster.Detail
Provides detailed information about guildmembers.
 
    detail = Inspect.Guild.Roster.Detail(guildmember)   -- table <- string
    details = Inspect.Guild.Roster.Detail(guildmembers)   -- table <- table
#### Parameters:
**guildmembers** - A table of names of guildmembers to retrieve data for.
**guildmember** - The name of the guildmember to retrieve data for.
#### Return Values:
**detail** - Detail table for a single guildmember.
**details** - Detail tables for all requested guildmembers. The key is the guildmember's name, the value is the guildmember's detail table.
#### Returned Members:
**level** - The guildmember's level.
**noteOfficer** - The guildmember's officer note.
**id** - The ID of the requested element.
**status** - The guildmember's current status. May include "online" or "mobile". nil if the guildmember is offline.
**rank** - The guildmember's rank.
**zone** - The guildmember's current zone, if online.
**logout** - The guildmember's last logout time, in UNIX time.
**note** - The guildmember's public note.
**name** - The guildmember's name.
**score** - The guildmember's achievement score.
### Inspect.Guild.Roster.List
Lists guildmembers.
 
    list = Inspect.Guild.Roster.List()   -- table <- void
#### Return Values:
**list** - A table listing all your guildmembers. The key is the guildmember's name, the value is the guildmember's status.
### Inspect.Interaction
Provides information about what types of interaction are available.
 
    interactions = Inspect.Interaction()   -- table <- void
    status = Inspect.Interaction(interaction)   -- boolean <- string
#### Parameters:
**interaction** - Optional name of the interaction type to query. Valid values include "auction", "bank", "guildbank", and "mail".
#### Return Values:
**status** - Whether or not that interaction type is available.
**interactions** - Table from interaction type to indicator of whether that interaction is available.
### Inspect.Item.Detail
Provides detailed information about items.
 
    item = Inspect.Item.Detail(item)   -- table <- item
    item = Inspect.Item.Detail(itemtype)   -- table <- itemtype
    item = Inspect.Item.Detail(slot)   -- table <- slot
    items = Inspect.Item.Detail(slot)   -- table <- slot
    items = Inspect.Item.Detail(elements)   -- table <- table
#### Parameters:
**slot** - A single slot specifier.
**item** - A single item ID.
**elements** - A table of slot specifiers, item IDs, or item types.
**itemtype** - A single item type.
#### Return Values:
**items** - Detail tables for all requested items. The key is the string used to lookup, the value is the item's detail table.
**item** - Detail table for a single item.
#### Returned Members:
**category** - The item's type category.
**requiredSkillLevel** - The skill level required to use this item.
**mountAutoscale** - Indicates if the mount autoscales to your fastest mount speed.
**requiredFaction** - The ID of the faction required to use this item.
**statsRune** - The added rune stats of this item. May contain the same members as stats.
**description** - The description of this item.
**damageMax** - If a weapon, the maximum damage done by a single hit with this item.
**coin** - The amount of silver this item represents.
**requiredPrestige** - The prestige rank required to use this item.
**cooldownDuration** - Duration of the current cooldown the item is influenced by, in seconds.
**requiredFactionLevel** - The faction notoriety required to use this item.
**stackMax** - The maximum size of this item stack.
**crafter** - The name of the player who crafted this item.
**damageType** - If a weapon, the damage type done by autoattacks. Values include "life", "death", "air", "earth", "fire", and "water".
**id** - The ID of the requested element.
**statsRuneTemporary** - The added temporary rune stats of this item. May contain the same members as stats.
**fragmentAffinity** - The planar type of the planar fragment.
**cooldownRemaining** - Time remaining in the item's current cooldown, in seconds.
**type** - The item's type specifier.
**requiredLevel** - The level required to use this item.
**stats** - The base stats of this item. Members may include "armor", "block", "critAttack", "critPower", "critSpell", "dexterity", "dodge", "endurance", "energyMax", "energyRegen", "hit", "intelligence", "manaMax", "manaRegen", "movement", "powerAttack", "powerMax", "powerRegen", "powerSpell", "resistAir", "resistAll", "resistDeath", "resistEarth", "resistFire", "resistLife", "resistWater", "stealth", "stealthDetect", "strength", and "wisdom".
**cooldown** - The cooldown for using this item.
**slots** - If a container, the number of slots that this item can contain.
**sell** - The sell value of this item, in silver.
**requiredSkill** - The skill required to use this item.
**rarity** - The item's rarity. Values include "sellable", "uncommon", "rare", "epic", "relic", "transcendent", or "quest". Common items have a rarity of nil.
**range** - If a ranged weapon, the maximum range of this item.
**bound** - The item's bound flag.
**bind** - The item's binding type. May be "equip", "use", "pickup", or "account".
**icon** - Resource filename of the item's icon.
**cooldownExpired** - Number of seconds the current cooldown is past its expiration time. Generally indicates lag.
**stack** - The size of this item stack.
**damageMin** - If a weapon, the minimum damage done by a single hit with this item.
**cooldownBegin** - The time the current cooldown started, in the context of Inspect.Time.Frame.
**flavor** - The flavor text for this item.
**infusionLevel** - The current level of a planar fragment.
**mountAquatic** - Indicates that the mount is aquatic.
**lootable** - Indicates that the item contains loot.
**fragmentTier** - The pairing rating of the stats for the planar fragment.
**requiredCalling** - Space-delimited list of the required callings to use this item.
**mountSpeed** - The speed of your character while mounted.
**name** - The item's name.
**damageDelay** - If a weapon, the delay between autoattacks using this weapon.
### Inspect.Item.Find
Finds a slot specifier based on an item ID.
 
    slot = Inspect.Item.Find(item)   -- slot <- item
    slots = Inspect.Item.Find(items)   -- table <- table
#### Parameters:
**items** - A table of item IDs.
**item** - A single item ID.
#### Return Values:
**slot** - A slot specifier for that item.
**slots** - Slot specifiers for all requested items. The key is the string used to lookup, the value is the slot specifier.
### Inspect.Item.List
Generate a list of item IDs from a slot specifier or set of slot specifiers.
 
    items = Inspect.Item.List()   -- table <- void
    item = Inspect.Item.List(slot)   -- item <- slot
    items = Inspect.Item.List(slot)   -- table <- slot
    items = Inspect.Item.List(slots)   -- table <- table
#### Parameters:
**slot** - A single slot specifier.
**slots** - A table of slot specifiers.
#### Return Values:
**items** - A table of item IDs. The key is the slot specifier, the value is the item ID.
**item** - A single item ID. This will be returned only if the input is a single fully-specified slot specifier.
### Inspect.Item.Mount.List
Returns details about all of your collected mounts.
 
    mounts = Inspect.Item.Mount.List()   -- table <- void
#### Return Values:
**mounts** - Valid collected mount, in {id = "type"} format.
### Inspect.Mail.Detail
Returns information about mail.
 
    detail = Inspect.Mail.Detail(mail)   -- table <- mail
    details = Inspect.Mail.Detail(mails)   -- table <- table
#### Parameters:
**mails** - A table of identifiers of mail to retrieve detail for.
**mail** - The identifier of the mail to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single mail.
**details** - Detail tables for all requested mail. The key is the mail ID, the value is the mail's detail table.
#### Returned Members:
**id** - The ID of the requested element.
**read** - "true" if you have already opened this mail.
**expire** - The time this mail will expire, in Unix timestamp form.
**spam** - "true" if this mail is considered spam.
**subject** - The subject line for this mail.
**attachments** - The attachments available on this mail. A number if this mail has basic information, or a table of item IDs if this mail has detailed information.
**from** - The name of the character this mail was sent from.
**body** - The body of this mail. Available only if detailed information on the mail has been retrieved.
**cod** - The Cash on Delivery required to retrieve attachments out of this mail message.
#### Restrictions:
Permitted only with the 'mail' interaction active.
### Inspect.Mail.List
Returns a table of valid mail and its status.
 
    mails = Inspect.Mail.List()   -- table <- void
#### Return Values:
**mails** - Valid mail, in {id = "type"} format. The type may be either "basic" or "detail", indicating whether the mail has been opened and its detailed information is available.
#### Restrictions:
Permitted only with the 'mail' interaction active.
### Inspect.Map.Detail
Returns information about map locations.
 
    detail = Inspect.Map.Detail(location)   -- table <- location
    details = Inspect.Map.Detail(locations)   -- table <- table
#### Parameters:
**location** - The identifier of the location to retrieve detail for.
**locations** - A table of identifiers of locations to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single location.
**details** - Detail tables for all requested locations. The key is the location ID, the value is the location's detail table.
#### Returned Members:
**coordY** - The map location's current Y coordinate.
**title** - The title of this map location, if it has one.
**id** - The ID of the requested element.
**coordZ** - The map location's current Z coordinate.
**coordX** - The map location's current X coordinate.
**description** - The description of this map location, if it has one.
### Inspect.Map.List
Returns a table of map locations.
 
    list = Inspect.Map.List()   -- table <- void
#### Return Values:
**list** - Valid locations, in {id = true} format.
### Inspect.Map.Monitor
Inspects the state of the map monitor flag. See Command.Map.Monitor() for details.
 
    monitor = Inspect.Map.Monitor()   -- boolean <- void
#### Return Values:
**monitor** - The current state of the map monitor flag.
### Inspect.Map.Waypoint.Get
Gets a unit's current waypoint.
 
    x, z = Inspect.Map.Waypoint.Get(unit)   -- number, number <- unit
#### Parameters:
**unit** - A unit, in either unit ID or unit specifier format.
#### Return Values:
**z** - The z coordinate of the waypoint.
**x** - The x coordinate of the waypoint.
### Inspect.Message.Accept.Check
Checks whether a given message type would be accepted if it were sent to you. Parameters can be replaced by "nil" to check for wildcard acceptance.
 
    accepted = Inspect.Message.Accept.Check(type, identifier)   -- boolean <- string/nil, string/nil
#### Parameters:
**identifier** - The identifier type of the message. Used for the receiver to filter accepted messages via the Command.Message.Accept() function. Must be at least three characters long.
**type** - The type of message. Valid types include "tell", "channel", "guild", "officer", "party", "raid", "say", "yell", "send".
#### Return Values:
**accepted** - True if the message would be transferred to this client.
### Inspect.Message.Accept.List
Retrieves the list of accepted message types and identifiers.
 
    accepts = Inspect.Message.Accept.List()   -- table <- void
#### Return Values:
**accepts** - List of all accepted message types. Takes the form of a key/value table. The key is a table containing {type, identifier}, where either element may be nil to indicate a wildcard. The value is the number of times this type has been accepted.
### Inspect.Minion.Adventure.Detail
Provides detailed information about minion adventures.
 
    detail = Inspect.Minion.Adventure.Detail(adventure)   -- table <- minionadventure
    details = Inspect.Minion.Adventure.Detail(adventures)   -- table <- table
#### Parameters:
**adventures** - A table of identifiers of minion adventures to retrieve detail for.
**adventure** - The identifier of the minion adventure to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single minion adventure.
**details** - Detail table for all requested minion adventures. The key is the minion adventure ID, the value is the adventure's detail table.
#### Returned Members:
**statAssassination** - Indicates greater rewards if finished by a minion with an Assassination stat.
**statHunting** - Indicates greater rewards if finished by a minion with a Hunting stat.
**costCredit** - The cost to start this adventure in credits.
**id** - The ID of the requested element.
**statWater** - Indicates greater rewards if finished by a minion with a Water stat.
**statExploration** - Indicates greater rewards if finished by a minion with an Exploration stat.
**statFire** - Indicates greater rewards if finished by a minion with a Fire stat.
**statDiplomacy** - Indicates greater rewards if finished by a minion with a Diplomacy stat.
**completion** - The time a "working" adventure will change to "finished", represented as UNIX time.
**statLife** - Indicates greater rewards if finished by a minion with a Life stat.
**statAir** - Indicates greater rewards if finished by a minion with an Air stat.
**hurryCredit** - The cost to hurry this adventure in credits.
**duration** - The duration of this adventure in seconds.
**statEarth** - Indicates greater rewards if finished by a minion with an Earth stat.
**hurryAventurine** - The cost to hurry this adventure in Aventurine.
**statDeath** - Indicates greater rewards if finished by a minion with a Death stat.
**statArtifact** - Indicates greater rewards if finished by a minion with an Artifact stat.
**statDimension** - Indicates greater rewards if finished by a minion with a Dimension stat.
**reward** - The reward type of this adventure.
**rewardQuality** - The final reward quality of this adventure once in "finished" mode.
**costStamina** - The cost to start this adventure in stamina.
**minion** - The minion currently involved with this adventure.
**mode** - The mode this adventure is in. One of "available", "working", or "finished".
**statHarvesting** - Indicates greater rewards if finished by a minion with a Harvesting stat.
**name** - The name of this adventure.
**costAventurine** - The cost to start this adventure in Aventurine.
### Inspect.Minion.Adventure.List
Lists known minion adventures.
 
    list = Inspect.Minion.Adventure.List()   -- table <- void
#### Return Values:
**list** - A table of minion adventure IDs. The key is the minion adventure ID.
### Inspect.Minion.Minion.Detail
Provides detailed information about minions.
 
    detail = Inspect.Minion.Minion.Detail(minion)   -- table <- minionminion
    details = Inspect.Minion.Minion.Detail(minions)   -- table <- table
#### Parameters:
**minions** - A table of identifiers of minions to retrieve detail for.
**minion** - The identifier of the minion to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single minion.
**details** - Detail table for all requested minions. The key is the minion ID, the value is the minion's detail table.
#### Returned Members:
**statAssassination** - This minion's Assassination stat.
**statHunting** - This minion's Hunting stat.
**id** - The ID of the requested element.
**statWater** - This minion's Water stat.
**statExploration** - This minion's Exploration stat.
**statFire** - This minion's Fire stat.
**statDiplomacy** - This minion's Diplomacy stat.
**experienceNeeded** - If not at max level, the amount of experience required to gain a level.
**statLife** - This minion's Life stat.
**staminaMax** - The maximum stamina of this minion.
**rarity** - The rarity of this minion. "common", "uncommon", "rare", or "epic".
**statAir** - This minion's Air stat.
**level** - The current level of this minion.
**statArtifact** - This minion's Artifact stat.
**statDimension** - This minion's Dimension stat.
**statDeath** - This minion's Death stat.
**experienceAccumulated** - If not at max level, this minion's accumulated experience in their current level.
**description** - The description of this minion.
**stamina** - The current stamina of this minion.
**statHarvesting** - This minion's Harvesting stat.
**name** - The name of this minion.
**statEarth** - This minion's Earth stat.
### Inspect.Minion.Minion.List
Lists available minions.
 
    list = Inspect.Minion.Minion.List()   -- table <- void
#### Return Values:
**list** - A table of minion IDs. The key is the minion ID.
### Inspect.Minion.Slot
Returns the total number of minion adventure slots.
 
    slots = Inspect.Minion.Slot()   -- number <- void
#### Return Values:
**slots** - The number of slots available for active minion adventures.
### Inspect.Mouse
Returns information about the current mouse position and button state.
 
    results = Inspect.Mouse()   -- table <- void
#### Return Values:
**results** - Table containing the current mouse state. May include members x, y, Left, Right, Middle, Mouse4, Mouse5.
### Inspect.Pvp.History
Returns information on your PVP history.
 
    result = Inspect.Pvp.History()   -- table <- void
#### Return Values:
**result** - Table containing information on PVP history.
#### Returned Members:
**killLifetime** - Amount of kills made on this character.
**favorLifetime** - Amount of lifetime favor accumulated.
**killToday** - Amount of kills made on this character within the last day.
**killMonth** - Amount of kills made on this character within the last month.
**killWeek** - Amount of kills made on this character within the last week.
### Inspect.Pvp.Prestige
Returns information on your PVP prestige.
 
    result = Inspect.Pvp.Prestige()   -- table <- void
#### Return Values:
**result** - Table containing information on prestige.
#### Returned Members:
**needed** - Quantity of prestige needed to gain a rank.
**rank** - Current prestige rank.
**accumulated** - Prestige accumulated so far in this rank.
### Inspect.Quest.Complete
Returns a table of all quests completed.
 
    quests = Inspect.Quest.Complete()   -- table <- void
#### Return Values:
**quests** - A table where the key is the incomplete quest identifier. Identifiers returned by this function may not include all characters - the last eight characters may be replaced by "xxxxxxxx". These elements can still be compared against standard quest IDs by comparing only the first nine characters (including the 'q' prefix).
### Inspect.Quest.Detail
Provides detailed information about a quest.
 
    detail = Inspect.Quest.Detail(quest)   -- table <- quest
    details = Inspect.Quest.Detail(quests)   -- table <- table
#### Parameters:
**quest** - The quest to retrieve detail for.
**quests** - A table of quests to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single quest.
**details** - Detail tables for all requested quests.
#### Returned Members:
**domain** - The quest's domain. May be "area", "guild", "instant", or "zone". Personal quests are signaled with nil.
**id** - The ID of the requested element.
**rewardExperienceGuild** - The amount of guild experience this quest's completion will award.
**rewardGuaranteed** - Lists the guaranteed rewards for the player. The key is the item type, the value is the count.
**rewardExperience** - The amount of experience this quest's completion will award.
**track** - Signals that this quest is being manually tracked.
**rewardPrestige** - The amount of prestige this quest's completion will award.
**complete** - Signals that this quest is complete.
**rewardChoose** - Lists the possible reward choices for the player. The key is the item type, the value is the count.
**rewardFavor** - The amount of favor this quest's completion will award.
**summary** - The short summary for this quest.
**objective** - A table of objectives. Each objective is a table that may contain the following members: "description", representing the quest's full description. "count", representing how many elements must be completed to finish this quest. "countDone", representing how many elements have been completed. "complete", indicating that this objective is complete. "indicator", listing all the map indicators that can be used to complete this quest.
**categoryName** - The name of the category this quest is placed under.
**mode** - This quest's mode, if it has a special mode. May be "story" or "soul".
**watch** - Signals that this quest is being watched.
**tagName** - The quest's tags, localized.
**tag** - The quest's tags, space-separated.
**description** - The long description for this quest.
**rewardNotoriety** - The amount of notoriety this quest's completion will award, represented as a table. The key is the faction ID, the value is the amount.
**rewardCoin** - The amount of silver this quest's completion will award.
**name** - The name of this quest.
**failed** - Signals that this quest has failed.
### Inspect.Quest.List
Lists the player's current active quests.
 
    quests = Inspect.Quest.List()   -- table <- void
#### Return Values:
**quests** - A table containing the player's current quests.
### Inspect.Queue.Handler
Returns the current queue handler function.
 
    handler, owner = Inspect.Queue.Handler()   -- function, string <- void
#### Return Values:
**handler** - The current queue handler function.
**owner** - The owner of the current queue handler function.
### Inspect.Queue.Status
Inspects the current queue status. Omit the first parameter to get a table containing information on all queues.
 
    results = Inspect.Queue.Status()   -- table <- void
    result = Inspect.Queue.Status(queue)   -- boolean <- string
    results = Inspect.Queue.Status(queue, size)   -- table <- string, number
#### Parameters:
**queue** - Optional identifier of the queue to query. Valid values include "global" and "auctionfullscan".
**size** - The size of the element to test in this queue. Defaults to 1 if no size is provided.
#### Return Values:
**result** - false if this queue is throttled, non-false otherwise.
**results** - A key/value table. The first parameter is the name of the queue. The second parameter is false if the queue is throttled, or a number representing the queue size available otherwise.
### Inspect.Role.List
Lists the player's roles.
 
    roles = Inspect.Role.List()   -- table <- void
#### Return Values:
**roles** - A table containing the player's roles. The key is the role ID, the value is the role name.
### Inspect.Setting.Detail
Provides detailed information about a setting.
 
    detail = Inspect.Setting.Detail(setting)   -- table <- string
    details = Inspect.Setting.Detail(settings)   -- table <- table
#### Parameters:
**setting** - The setting to retrieve detail for.
**settings** - A table of settings to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single setting.
**details** - Detail tables for all requested settings.
#### Returned Members:
**value** - The value of the requested element.
**id** - The ID of the requested element.
m] has successfully crafted [Carmintium Lariat]!
### Inspect.Setting.List
Lists the player's current settings.
 
    settings = Inspect.Setting.List()   -- table <- void
#### Return Values:
**settings** - A table containing the player's current settings.
### Inspect.Shard
Returns information about the current shard.
 
    shard = Inspect.Shard()   -- table <- void
#### Return Values:
**shard** - Table containing requested data.
#### Returned Members:
**rp** - Signals that the shard is a roleplaying shard.
**name** - The shard's name.
**pvp** - Signals that the shard is a PvP shard.
### Inspect.Social.Friend.Detail
Provides detailed information about a friend.
 
    detail = Inspect.Social.Friend.Detail(friend)   -- table <- string
    details = Inspect.Social.Friend.Detail(friends)   -- table <- table
#### Parameters:
**friend** - The name of a friend to retrieve detail for.
**friends** - A table of names of friends to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single friend.
**details** - Detail tables for all requested friends.
#### Returned Members:
**calling** - The friend's calling, if known.
**note** - The friend's personal note.
**status** - The friend's status. Either "online", "afk", or nil.
**race** - The friend's race, if known.
**zone** - The friend's current zone, if known.
**guild** - The friend's current guild.
**level** - The friend's current level, if known.
**name** - The friend's name.
**id** - The ID of the requested element.
### Inspect.Social.Friend.List
Lists the player's current friends.
 
    friends = Inspect.Social.Friend.List()   -- table <- void
#### Return Values:
**friends** - A table containing the player's current friends.
### Inspect.Social.Ignore.Detail
Provides detailed information about an ignored player.
 
    detail = Inspect.Social.Ignore.Detail(ignore)   -- table <- string
    details = Inspect.Social.Ignore.Detail(ignores)   -- table <- table
#### Parameters:
**ignores** - A table of names of ignored players to retrieve detail for.
**ignore** - The name of an ignored player to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single ignored player.
**details** - Detail tables for all requested ignored players.
#### Returned Members:
**note** - The ignored player's personal note.
**name** - The ignored player's name.
**id** - The ID of the requested element.
### Inspect.Social.Ignore.List
Lists the player's current ignored players.
 
    ignores = Inspect.Social.Ignore.List()   -- table <- void
#### Return Values:
**ignores** - A table containing the player's current ignored players.
### Inspect.Stat
Returns information about the player's stats.
 
    results = Inspect.Stat()   -- table <- void
    result = Inspect.Stat(stat)   -- number <- string
#### Parameters:
**stat** - Optional parameter identifying the stat desired. May be any of strength, dexterity, intelligence, wisdom, endurance, resistLife, resistDeath, resistFire, resistWater, resistEarth, resistAir, armor, powerAttack, critAttack, hit, powerSpell, critSpell, critPower, block, dodge, or any of the previous with "Unbuffed" appended.
#### Return Values:
**result** - The current value of that stat.
**results** - A table from stat ID to stat value, containing all the player's stats.
### Inspect.Storage.Used
Returns the storage space currently used for a given segment.
 
    current, maximum = Inspect.Storage.Used(segment)   -- number, number <- string
#### Parameters:
**segment** - The storage segment to access. "player" will access the target's per-player storage, "guild" will access the target's guild's per-guild storage. If this function has no target parameter, then the player will be targeted.
#### Return Values:
**maximum** - Maximum storage space available.
**current** - Current storage space used.
### Inspect.System.Error.Detail
Provides detailed information about an addon error.
 
    detail = Inspect.System.Error.Detail(error)   -- table <- error
    details = Inspect.System.Error.Detail(errors)   -- table <- table
#### Parameters:
**error** - The error to be inspected.
**errors** - A table of errors to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single error. Member documentation can be found in Event.System.Error.
**details** - Detail tables for all requested errors.
### Inspect.System.Language
Returns the client's current language.
 
    language = Inspect.System.Language()   -- string <- void
#### Return Values:
**language** - Current language. Valid values include "English", "French", "German", "Korean", "Russian", "Chinese", and "Taiwanese".
### Inspect.System.Secure
Returns the client's current secure mode.
 
    secure = Inspect.System.Secure()   -- boolean <- void
#### Return Values:
**secure** - The current secure mode.
### Inspect.System.Version
Returns information on the client version.
 
    version = Inspect.System.Version()   -- table <- void
#### Return Values:
**version** - A table containing detailed version information.
#### Returned Members:
**external** - Client external version.
**build** - Client build information.
**internal** - Client internal version.
### Inspect.System.Watchdog
Returns the number of seconds until the system watchdog may begin generating warnings.
 
    time = Inspect.System.Watchdog()   -- number <- void
#### Return Values:
**time** - Time left in seconds.
### Inspect.TEMPORARY.Experience
Returns information about the player's experience.
 
    accumulated, rested, needed = Inspect.TEMPORARY.Experience()   -- number, number, number <- void
#### Return Values:
**needed** - The amonut of experience required to reach the next level.
**accumulated** - The amount of experience that has accumulated towards the next level.
**rested** - The amount of rested experience that has accumulated.
#### Restrictions:
This function is deprecated and will be removed in the future. It should not be used.
### Inspect.TEMPORARY.Role
Returns the ID of the player's current role.
 
    role = Inspect.TEMPORARY.Role()   -- number <- void
#### Return Values:
**role** - The ID of the player's current role.
#### Restrictions:
This function will be removed in the future.
### Inspect.Time.Frame
The game time of the last frame. This function's return value will not change until the next frame.
 
    time = Inspect.Time.Frame()   -- number <- void
#### Return Values:
**time** - Time in seconds. Counted from an arbitrary point in the past. Guaranteed to be non-negative.
### Inspect.Time.Real
A high-resolution realtime timer. Not measured in the same timespace as Inspect.Time.Frame.
 
    time = Inspect.Time.Real()   -- number <- void
#### Return Values:
**time** - Time in seconds. Counted from an arbitrary point in the past. Guaranteed to be non-negative.
### Inspect.Time.Server
Returns the current server time.
 
    time = Inspect.Time.Server()   -- number <- void
#### Return Values:
**time** - Server time, as seconds from the Unix epoch.
### Inspect.Title.Category.Detail
Provides detailed information about a title category.
 
    detail = Inspect.Title.Category.Detail(titlecategory)   -- table <- titlecategory
    details = Inspect.Title.Category.Detail(titlecategories)   -- table <- table
#### Parameters:
**titlecategories** - A table of title categories to retrieve details for.
**titlecategory** - The title category to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single title category.
**details** - Detail tables for all requested title categories.
#### Returned Members:
**name** - The name of the title category.
**id** - The ID of the required element.
### Inspect.Title.Category.List
Lists known title categories.
 
    titlecategories = Inspect.Title.Category.List()   -- table <- void
#### Return Values:
**titlecategories** - A lookup table of known title categories.
### Inspect.Title.Detail
Provides detailed information about a title.
 
    detail = Inspect.Title.Detail(title)   -- table <- title
    details = Inspect.Title.Detail(titles)   -- table <- table
#### Parameters:
**title** - The title ID to retrieve detail for.
**titles** - A table of title IDs to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single title.
**details** - Detail tables for all requested titles.
#### Returned Members:
**nameMale** - The title as seen in menus and applied to male characters.
**gameMale** - The title as seen ingame and applied to male characters.
**nameFemale** - The title as seen in menus and applied to female characters.
**gameFemale** - The title as seen ingame and applied to female characters.
**prefix** - Signals that the title is a prefix title.
**id** - The ID of the requested element.
### Inspect.Title.List
Lists the player's currently available titles.
 
    titles = Inspect.Title.List()   -- table <- void
#### Return Values:
**titles** - A lookup table of all available titles.
### Inspect.Tooltip
Returns the current contents of the tooltip.
 
    type, shown = Inspect.Tooltip()   -- nil, nil <- void
    type, shown = Inspect.Tooltip()   -- string, variant <- void
    type, unit, buff = Inspect.Tooltip()   -- string, unit, buff <- void
#### Return Values:
**buff** - The ID of the tooltip's buff.
**unit** - The unit that the buff is attached to.
**shown** - The blob describing what is currently shown. Generally, some kind of identifier used in another part of the addon system.
**type** - The current tooltip type. Valid values include "ability", "buff", "item", "itemtype", and "unit".
### Inspect.Unit.Castbar
Provides detailed information about a unit's castbar.
 
    detail = Inspect.Unit.Castbar(unit)   -- table <- unit
    details = Inspect.Unit.Castbar(units)   -- table <- table
#### Parameters:
**unit** - A unit, in either unit ID or unit specifier format.
**units** - A table containing units to inspect.
#### Return Values:
**detail** - Detail table for a single castbar.
**details** - Detail tables for all requested castbars. The key is the unit ID or unit specifier, the value is the castbar's detail table.
#### Returned Members:
**expired** - Number of the seconds the cast is past its completion time. Generally indicates lag.
**remaining** - Time remaining on the cast, in seconds.
**begin** - The time the cast started, in the context of Inspect.Time.Frame.
**duration** - Duration of the cast in seconds.
**uninterruptible** - Signals that the cast is not interruptible.
**ability** - ID of the ability being cast, if available.
**abilityName** - Name of the ability being cast.
**channeled** - True if this ability is channeled.
### Inspect.Unit.Detail
Provides detailed information about a unit.
 
    detail = Inspect.Unit.Detail(unit)   -- table <- unit
    details = Inspect.Unit.Detail(units)   -- table <- table
#### Parameters:
**unit** - A unit, in either unit ID or unit specifier format.
**units** - A table containing units to inspect.
#### Return Values:
**detail** - Detail table for a single unit.
**details** - Detail tables for all requested units. The key is the unit ID or unit specifier, the value is the unit's detail table.
#### Returned Members:
**mana** - The unit's mana.
**blocked** - Signals that this unit is not in line of sight. Provided only for groupmembers.
**loot** - The Unit ID that has looting rights to this corpse.
**zone** - The ID of the unit's current zone.
**calling** - The unit's calling. May be "mage", "rogue", "cleric", "warrior", or "primalist".
**vitality** - The unit's vitality. Provided only for the player or groupmembers.
**role** - The unit's role. May be "tank", "heal", "dps", "support", or nil. Provided only for the player and the player's groupmembers.
**absorb** - The unit's damage absorption.
**type** - The unit type ID.
**coordZ** - The unit's current Z coordinate.
**titleSuffixId** - The unit's title suffix ID.
**energy** - The unit's energy.
**titlePrefixName** - The unit's localized title prefix.
**planar** - The unit's available planar charges. Provided only for the player or groupmembers.
**guaranteedLoot** - Signals that this unit guarantees loot on death. Shown in the user interface as a diamond above the portrait.
**nameSecondary** - The unit's secondary name.
**titlePrefixId** - The unit's title prefix ID.
**offline** - Signals that the unit is offline. Provided only for the player's groupmembers.
**healthMax** - The unit's maximum health.
**tier** - The unit's difficulty tier. nil, "group", or "raid".
**health** - The unit's health.
**healthCap** - The unit's capped maximum health.
**pvp** - The unit's PvP flag.
**publicSize** - The unit's current public group size. nil if the group is not public. Provided only for friendly players.
**spirit** - The unit's spirit.
**locationName** - The name of the unit's location. Provided only for friendly players.
**tag** - The unit's tags, space-separated.
**tagName** - The unit's tags, localized.
**alliance** - The unit's alliance. "defiant", "guardian", or nil.
**race** - The unit's race. Provided only for players.
**mentoring** - The unit's mentoring status.
**tagged** - The unit's tagged status. true if the unit has been tagged by you, "other" if the unit has been tagged by someone else.
**manaMax** - The unit's maximum mana.
**radius** - The unit's radius.
**factionId** - The unit's faction ID.
**player** - Signals that the unit is a player, not an NPC.
**relation** - The unit's relation to you. May be "hostile" or "friendly". Neutral targets will not have this member.
**combo** - The unit's combo points. Provided only for the player.
**planarMax** - The unit's maximum planar charges. Provided only for the player or groupmembers.
**titleSuffixName** - The unit's localized title suffix.
**id** - The ID of the requested element.
**coordX** - The unit's current X coordinate.
**afk** - Signals that the unit is AFK. Provided only for the player and the player's groupmembers.
**level** - The unit's level. May be "??" if the unit is hostile and very high-level.
**aggro** - Signals that this unit is being attacked. Provided only for groupmembers.
**combat** - The unit's combat status.
**power** - The unit's power.
**chargeMax** - The unit's maximum charge. Provided only for the player.
**charge** - The unit's charge. Provided only for the player.
**raceName** - The unit's race, localized. Provided only for players.
**guild** - The unit's guild.
**coordY** - The unit's current Y coordinate.
**focus** - The unit's focus.
**energyMax** - The unit's maximum energy.
**warfront** - Signals that the unit has temporarily left the group to join a warfront. Provided only for groupmembers.
**ready** - The unit's readycheck status.
**availability** - The unit's current availability. See Utility.Unit.Availability() for details.
**name** - The unit's name.
**mark** - The mark on this unit.
### Inspect.Unit.List
Lists all the units that the client can see.
 
    list = Inspect.Unit.List()   -- table <- void
#### Return Values:
**list** - Map of unit ID to unit specifier. Units with multiple valid specifiers will have one chosen at random.
### Inspect.Unit.Lookup
Converts unit IDs to unit specifiers and vice-versa.
 
    unit = Inspect.Unit.Lookup(unit)   -- unit <- unit
    units = Inspect.Unit.Lookup(units)   -- table <- table
#### Parameters:
**unit** - A single unit ID or unit specifier.
**units** - A table containing unit IDs and unit specifiers.
#### Return Values:
**unit** - A unit ID or unit specifier, whichever is the opposite of the parameter given. May be nil.
**units** - A table of unit IDs and unit specifiers. The key is the input, the value is the result. Invalid inputs will not result in output entries.
### Inspect.Zone.Detail
Returns information about zones.
 
    detail = Inspect.Zone.Detail(zone)   -- table <- zone
    details = Inspect.Zone.Detail(zones)   -- table <- table
#### Parameters:
**zones** - A table of identifiers of zones to retrieve detail for.
**zone** - The identifier of the zone to retrieve detail for.
#### Return Values:
**detail** - Detail table for a single zone.
**details** - Detail tables for all requested zones. The key is the zone ID, the value is the zone's detail table.
#### Returned Members:
**type** - The type of this zone. May be either "instance", "warfront", or nil.
**name** - The name of the zone.
**id** - The ID of the requested element.
## Utilities
### Utility.Auction.Cost
Returns the amount of silver it will cost to post a given auction.
 
    cost = Utility.Auction.Cost(item, time, bid, buyout)   -- number <- item, number, number/nil, number/nil
    cost = Utility.Auction.Cost(item, time, bid, buyout, partial)   -- number <- item, number, number/nil, number/nil, boolean
#### Parameters:
**partial** - Whether partial buyouts should be permitted. Must be "false" if a buyout is not provided or if a bid is provided that is different from the buyout. If this parameter is omitted, will default to "true" when possible, or "false" otherwise. 
**time** - The duration that the auction should last, in hours. Valid values are limited to 12, 24, and 48.
**item** - The ID of the item to be auctioned.
**bid** - The minimum bid for the new auction, in silver. nil if no bid is desired.
**buyout** - The buyout for the new auction, in silver. nil if no buyout is desired.
#### Return Values:
**cost** - The cost of posting this auction, in silver.
#### Restrictions:
This function is deprecated and will be removed in the future. It should not be used.
### Utility.Auction.CostOrder
Returns the amount of silver it will cost to post a given order.
 
    cost = Utility.Auction.CostOrder(itemtype, quantity, time, buyout)   -- number <- itemtype, number, number, number
#### Parameters:
**quantity** - The number of items to be ordered.
**buyout** - The total buyout for the new auction, in silver. Must be evenly divisible by quantity.
**time** - The duration that the order should last, in hours. Valid values are limited to 12, 24, and 48.
**itemtype** - The item type to be ordered.
#### Return Values:
**cost** - The cost of posting this order, in silver. Includes the full deposit for the items.
### Utility.Auction.CostPost
Returns the amount of silver it will cost to post a given auction.
 
    cost = Utility.Auction.CostPost(item, time, bid, buyout)   -- number <- item, number, number/nil, number/nil
    cost = Utility.Auction.CostPost(item, time, bid, buyout, partial)   -- number <- item, number, number/nil, number/nil, boolean
#### Parameters:
**partial** - Whether partial buyouts should be permitted. Must be "false" if a buyout is not provided or if a bid is provided that is different from the buyout. If this parameter is omitted, will default to "true" when possible, or "false" otherwise. 
**time** - The duration that the auction should last, in hours. Valid values are limited to 12, 24, and 48.
**item** - The ID of the item to be auctioned.
**bid** - The minimum bid for the new auction, in silver. nil if no bid is desired.
**buyout** - The buyout for the new auction, in silver. nil if no buyout is desired.
#### Return Values:
**cost** - The cost of posting this auction, in silver.
### Utility.Dispatch
Calls a function, crediting that function's execution and errors to a specific addon. Errors will be handled by the standard error handler and not relayed to the caller. Does not return anything.
 
    Utility.Dispatch(func, identifier, info)   -- function, string, string
#### Parameters:
**func** - The function to call.
**identifier** - The identifier of the addon to credit execution time towards.
**info** - An info string to be included in error reports.
### Utility.Event.Create
Creates a custom event. Takes an identifier and an event path as parameters. Called with Create("Identifier", "The.Event.Path") the event table will end up at Event.Identifier.The.Event.Path, and will behave like a standard Rift event table in every way. Undefined behavior if given an identifier or a path that conflicts with a built-in Rift event or an existing addon event.
 
    caller, handle = Utility.Event.Create(identifier, event)   -- function, table <- string, string
#### Parameters:
**identifier** - The identifier of the addon creating this event.
**event** - The event's name. "." characters will be treated as hierarchy delimeters.
#### Return Values:
**caller** - A function used to trigger the event. Called with any selection of parameters, it will pass those parameters through to properly registered event handlers in order. Any errors caused by those event handlers will be caught and handled.
**handle** - The resulting event handle.
### Utility.Item.Slot.All
Generates a slot specifier for all known items.
 
    slot = Utility.Item.Slot.All()   -- slot <- void
#### Return Values:
**slot** - The requested slot specifier.
### Utility.Item.Slot.Bank
Generates a slot specifier for the player's bank.
 
    slot = Utility.Item.Slot.Bank()   -- slot <- void
    slot = Utility.Item.Slot.Bank(segment)   -- slot <- string
    slot = Utility.Item.Slot.Bank(segment)   -- slot <- number
    slot = Utility.Item.Slot.Bank(segment, slot)   -- slot <- string, number
    slot = Utility.Item.Slot.Bank(segment, slot)   -- slot <- number, number
#### Parameters:
**slot** - The slot ID, starting at 1.
**segment** - Segment to inspect. Use "main" for the main bank area, "bag" for the bank bags, or a number starting at 1 for the contents of a specific bank bag.
#### Return Values:
**slot** - The requested slot specifier.
### Utility.Item.Slot.Equipment
Generates a slot specifier for the player's equipment.
 
    slot = Utility.Item.Slot.Equipment()   -- slot <- void
    slot = Utility.Item.Slot.Equipment(slot)   -- slot <- string
#### Parameters:
**slot** - The equipment slot to be used. Can be any of the following: "helmet", "cape", "shoulders", "chest", "gloves", "belt", "legs", "feet", "handmain", "handoff", "ranged", "neck", "trinket", "ring1", "ring2", "synergy", "focus", or "seal".
#### Return Values:
**slot** - The requested slot specifier.
### Utility.Item.Slot.Fragments
Generates a slot specifier for the player's fragment inventory.
 
    slot = Utility.Item.Slot.Fragments()   -- slot <- void
    slot = Utility.Item.Slot.Fragments(slot)   -- slot <- number
#### Parameters:
**slot** - The slot ID, starting at 1.
#### Return Values:
**slot** - The requested slot specifier.
### Utility.Item.Slot.Guild
Generates a slot specifier for the player's guild bank.
 
    slot = Utility.Item.Slot.Guild()   -- slot <- void
    slot = Utility.Item.Slot.Guild(vault)   -- slot <- number
    slot = Utility.Item.Slot.Guild(vault, slot)   -- slot <- number, number
#### Parameters:
**slot** - The slot ID, starting at 1.
**vault** - The vault ID to inspect. Starts at 1.
#### Return Values:
**slot** - The requested slot specifier.
### Utility.Item.Slot.Inventory
Generates a slot specifier for the player's inventory.
 
    slot = Utility.Item.Slot.Inventory()   -- slot <- void
    slot = Utility.Item.Slot.Inventory(bag)   -- slot <- string
    slot = Utility.Item.Slot.Inventory(bag)   -- slot <- number
    slot = Utility.Item.Slot.Inventory(bag, slot)   -- slot <- string, number
    slot = Utility.Item.Slot.Inventory(bag, slot)   -- slot <- number, number
#### Parameters:
**slot** - The slot ID, starting at 1.
**bag** - The number of the bag whose contents should be inspected, starting at 1, or "bag" to inspect the actual inventory bags.
#### Return Values:
**slot** - The requested slot specifier.
### Utility.Item.Slot.Parse
Parses a slot specifier, returning information on what it represents.
 
    type, parameter, parameter = Utility.Item.Slot.Parse(slot)   -- string, variant, variant <- slot
    type, parameter = Utility.Item.Slot.Parse(slot)   -- string, variant <- slot
    type = Utility.Item.Slot.Parse(slot)   -- string <- slot
    Utility.Item.Slot.Parse(slot)   -- slot
#### Parameters:
**slot** - The slot ID, starting at 1.
#### Return Values:
**type** - The type of this element.
**parameter** - A parameter for this slot type. Actual meaning depends on the type.
### Utility.Item.Slot.Quest
Generates a slot specifier for the player's quest bag.
 
    slot = Utility.Item.Slot.Quest()   -- slot <- void
    slot = Utility.Item.Slot.Quest(slot)   -- slot <- number
#### Parameters:
**slot** - The slot ID, starting at 1.
#### Return Values:
**slot** - The requested slot specifier.
### Utility.Item.Slot.Vault
Generates a slot specifier for the player's bank vaults.
 
    slot = Utility.Item.Slot.Vault()   -- slot <- void
    slot = Utility.Item.Slot.Vault(vault)   -- slot <- number
    slot = Utility.Item.Slot.Vault(vault, slot)   -- slot <- number, number
#### Parameters:
**slot** - The slot ID, starting at 1.
**vault** - The vault ID, starting at 1.
#### Return Values:
**slot** - The requested slot specifier.
### Utility.Item.Slot.Wardrobe
Generates a slot specifier for the player's wardrobe.
 
    slot = Utility.Item.Slot.Wardrobe()   -- slot <- void
    slot = Utility.Item.Slot.Wardrobe(costume)   -- slot <- number
    slot = Utility.Item.Slot.Wardrobe(costume, slot)   -- slot <- number, string
#### Parameters:
**slot** - The equipment slot to be used. Can be any of the following: "helmet", "cape", "shoulders", "chest", "gloves", "legs", or "feet".
**costume** - The number of the wardrobe costume to inspect, starting at 1.
#### Return Values:
**slot** - The requested slot specifier.
### Utility.Mail.Cost
Returns the amount of silver it will cost to send a given mail.
 
    cost = Utility.Mail.Cost(mail)   -- number <- table
#### Parameters:
**mail** - The mail to check. In the same format as the parameter of Command.Mail.Send.
#### Return Values:
**cost** - The cost of sending this mail, in silver.
### Utility.Matrix.Create
Creates a transformation matrix table. These can also be created by hand; this is simply a convenience routine. Applies scaling first, then rotation, then translation.
 
    matrix = Utility.Matrix.Create(scaleX, scaleY, rotate, translateX, translateY)   -- table <- number/nil, number/nil, number/nil, number/nil, number/nil
#### Parameters:
**scaleY** - Scaling along the Y axis. Defaults to 1.
**scaleX** - Scaling along the X axis. Defaults to 1.
**rotate** - Rotation amount in radians. Defaults to 0.
**translateX** - Translation along the X axis. Defaults to 0.
**translateY** - Translation along the Y axis. Defaults to 0.
#### Return Values:
**matrix** - Matrix table suitable for "transform" members.
### Utility.Message.Limits
Returns information on the message bandwidth limits.
 
    data = Utility.Message.Limits()   -- table <- void
#### Return Values:
**data** - The requested data.
#### Returned Members:
**burst** - The amount of data that can be sent or received without being subject to throttling.
**maximum** - Maximum allowed message size.
**sustained** - Amount of data that can be transferred per second without dropping messages.
### Utility.Message.Size
Returns the size of a message, as used for bandwidth control.
 
    size = Utility.Message.Size(to, identifier, data)   -- number <- string/nil, string, string
#### Parameters:
**to** - The name of the player or channel to send to. nil if this is not targeted at a specific player or channel.
**data** - The data to send. This parameter is binary-safe.
**identifier** - The identifier type of the message. Used for the receiver to filter accepted messages via the Command.Message.Accept() function. Must be at least three characters long.
#### Return Values:
**size** - The calculated size of the message.
### Utility.Serialize.Full
Serializes a table of parameters. Results in a string suitable for feeding directly into loadstring(). Deals properly with table cycles and non-tree structures.
 
    serialized = Utility.Serialize.Full(elements)   -- string <- table
    serialized = Utility.Serialize.Full(elements, exists)   -- string <- table, table
#### Parameters:
**elements** - Table of elements to serialize.
**exists** - Optional parameter containing a table of elements that should be included even in the case that they are nil.
#### Return Values:
**serialized** - String containing serialized output. May be nil if there was an error with the input.
### Utility.Serialize.Inline
Serializes a single parameter. Results in a string suitable for use as a parameter in Lua code. If the input is a table, it must not contain table cycles or non-tree structures.
 
    serialized = Utility.Serialize.Inline(element)   -- string <- variant
#### Parameters:
**element** - Element to serialize.
#### Return Values:
**serialized** - String containing serialized output. May be nil if there was an error with the input.
### Utility.Storage.Checksum
Calculates the storage checksum of a string.
 
    checksum = Utility.Storage.Checksum(data)   -- string <- string
#### Parameters:
**data** - The data to store. This parameter is binary-safe.
#### Return Values:
**checksum** - An opaque string representing the storage checksum.
### Utility.Storage.Limits
Returns information on the storage bandwidth limits.
 
    data = Utility.Storage.Limits()   -- table <- void
#### Return Values:
**data** - The requested data.
#### Returned Members:
**burst** - The amount of data that can be sent or received without being subject to throttling.
**sustained** - Amount of data that can be transferred per second without dropping messages.
### Utility.Storage.Size
Returns the size of a stored element, as used for storage quota and bandwidth control.
 
    size = Utility.Storage.Size(identifier, data)   -- number <- string, string
#### Parameters:
**data** - The data to store. This parameter is binary-safe.
**identifier** - The identifier of the storage bucket. Must be at least three characters long.
#### Return Values:
**size** - The calculated size of the storage bucket.
### Utility.Type
Determines the type of a given item. Understands Rift-specific identifiers.
 
    type = Utility.Type(element)   -- string <- variant
#### Parameters:
**element** - The element to check.
#### Return Values:
**type** - The type of this element.
### Utility.Unit.Availability
Returns information on which detail members are available in which availability mode.
 
    lookup = Utility.Unit.Availability()   -- table <- void
#### Return Values:
**lookup** - Lookup table for which members are available in which availability mode. To check if "full" availability contains "level", check "lookup.full.level".
## UI
### UI.Context
Lists all contexts that have been created. Organizes them under [addonIdentifier][contextIdentifier][constructionOrder]. As an example, if TestAddon created four contexts named UIElement, the second would be UI.Context.TestAddon.UIElement[2].
### UI.CreateContext
Creates a new UI context. A UI context must be created in order to create any frames.
 
    context = UI.CreateContext(name)   -- Context <- string
#### Parameters:
**name** - A descriptive name for this element. Used for error reports and performance information. May be shown to end-users.
#### Return Values:
**context** - A new context. Contexts are guaranteed to have at least the capabilities of a Frame.
### UI.CreateFrame
Creates a new frame. Frames are the blocks that all addon UIs are made out of. Since all frames must have a parent, you'll need to create a Context before any frames. See UI.CreateContext.
List of valid frame types:
Frame: The base type. No special capabilities. Useful for spacing, organization, input handling, and solid color squares.
Mask: Obscures the contents of child frames that do not fall within the mask boundaries.
Text: Displays text.
Texture: Displays a static texture image.
RiftButton: A standard Rift button widget.
RiftCheckbox: A standard Rift checkbox widget.
RiftScrollbar: A standard Rift scrollbar widget.
RiftSlider: A standard Rift slider widget.
RiftTextfield: A standard Rift textfield widget.
RiftWindow: A standard Rift window widget.
 
    frame = UI.CreateFrame(type, name, parent)   -- Frame <- string, string, Element
#### Parameters:
**type** - The type of your new frame. Current supported types: Frame, Text, Texture.
**name** - A descriptive name for this element. Used for error reports and performance information. May be shown to end-users.
**parent** - The new parent for this frame.
#### Return Values:
**frame** - Your new frame.
### UI.Frame
Lists all frames that have been created. Organizes them under [addonIdentifier][contextIdentifier][constructionOrder]. As an example, if TestAddon created four frames named UIElement, the second would be UI.Frame.TestAddon.UIElement[2].
## UI Frames
### Canvas:ClearAll
Clear all set points and sizes from this frame.
 
    Frame:ClearAll()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Canvas:ClearHeight
Clear a set height from this frame.
 
    Frame:ClearHeight()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Canvas:ClearPoint
Clear a set point from this frame.
 
    Frame:ClearPoint(coordinate)   -- string
    Frame:ClearPoint(x, y)   -- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point.
**x** - X coordinate of the point.
**coordinate** - Named coordinate.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Canvas:ClearWidth
Clear a set width from this frame.
 
    Frame:ClearWidth()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Canvas:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Canvas:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
m] has successfully crafted [Carmintium Lariat]!
### Canvas:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### Canvas:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### Canvas:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### Canvas:GetAlpha
Gets the alpha multiplier of this frame.
 
    alpha = Element:GetAlpha()   -- number <- void
#### Return Values:
**alpha** - The alpha multiplier of this frame. 1 is fully opaque, 0 is fully transparent. This does not include multiplied alphas from this frame's parent - it's the exact value passed to SetAlpha.
### Canvas:GetBackgroundColor
Retrieves the background color of this frame.
 
    r, g, b, a = Element:GetBackgroundColor()   -- number, number, number, number <- void
#### Return Values:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.
**g** - Green.
### Canvas:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### Canvas:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### Canvas:GetChildren
Returns a table containing all of this element's children.
 
    children = Element:GetChildren()   -- table <- void
#### Return Values:
**children** - A table containing this element's children.
### Canvas:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### Canvas:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### Canvas:GetKeyFocus
Gets the key focus status.
 
    focus = Element:GetKeyFocus()   -- boolean <- void
#### Return Values:
**focus** - Whether this frame is the current key focus.
### Canvas:GetLayer
Gets the frame's layer order.
 
    layer = Frame:GetLayer()   -- number <- void
#### Return Values:
**layer** - The render layer of this frame. See Frame:SetLayer for details.
### Canvas:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### Canvas:GetMouseMasking
Get the current mouse masking mode. See SetMouseMasking for details.
 
    mask = Element:GetMouseMasking()   -- string <- void
#### Return Values:
**mask** - The current mouse masking mode.
### Canvas:GetMouseoverUnit
Gets the unit that is being represented by this frame.
 
    unit = Frame:GetMouseoverUnit()   -- string <- void
#### Return Values:
**unit** - The current mouseover unit. May be nil if there is no mouseover unit.
### Canvas:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
### Canvas:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### Canvas:GetParent
Gets the parent of this frame.
 
    parent = Frame:GetParent()   -- Element <- void
#### Return Values:
**parent** - The parent element of this frame.
### Canvas:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### Canvas:GetSecureMode
Get the current secure mode. See SetSecureMode for details.
 
    secure = Frame:GetSecureMode()   -- string <- void
#### Return Values:
**secure** - The current secure mode.
### Canvas:GetShape
Returns the current canvas shape.
 
    shape, fill, stroke = Canvas:GetShape()   -- table, table, table <- void
#### Return Values:
**stroke** - The current stroke style. See Canvas:SetShape() for details.
**fill** - The current fill style. See Canvas:SetShape() for details.
**shape** - The current shape. See Canvas:SetShape() for details.
### Canvas:GetStrata
Gets the frame's strata. The strata determines render order on a coarser level than Layer does, as well as determining how far an element is brought to the front when clicked on.
 
    strata = Frame:GetStrata()   -- string <- void
#### Return Values:
**strata** - The item's current strata.
### Canvas:GetStrataList
Gets a list of valid stratas for this frame.
 
    stratas = Frame:GetStrataList()   -- table <- void
#### Return Values:
**stratas** - An array of valid stratas, in order.
### Canvas:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### Canvas:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### Canvas:GetVisible
Gets the visibility flag for this frame.
 
    visible = Element:GetVisible()   -- boolean <- void
#### Return Values:
**visible** - This frame's visibility flag, as set by SetVisible. Does not check the visibility flags of the frame's parents.
### Canvas:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### Canvas:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### Canvas:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### Canvas:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### Canvas:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### Canvas:SetAllPoints
Pins all the edges of this frame to the edges of a different frame. If no target is given, defaults to this frame's parent.
 
    Frame:SetAllPoints()   -- void
    Frame:SetAllPoints(target)   -- Layout
#### Parameters:
**target** - Target Layout to pin this frame's edges to.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Canvas:SetAlpha
Sets the alpha transparency multiplier for this frame and its children.
 
    Element:SetAlpha(alpha)   -- number
#### Parameters:
**alpha** - The new alpha multiplier. 1 is fully opaque, 0 is fully transparent.
### Canvas:SetBackgroundColor
Sets the background color of this frame.
 
    Element:SetBackgroundColor(r, g, b)   -- number, number, number
    Element:SetBackgroundColor(r, g, b, a)   -- number, number, number, number
#### Parameters:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.
**g** - Green.
### Canvas:SetHeight
Sets the height of this frame. Undefined results if the frame already has two pinned Y coordinates.
 
    Frame:SetHeight(height)   -- number
#### Parameters:
**height** - The new height of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Canvas:SetKeyFocus
Sets the key focus status. Note that only one frame can be the key focus at a time. Focusing on another frame will automatically unset the current focus.
 
    Element:SetKeyFocus(focus)   -- boolean
#### Parameters:
**focus** - The new key focus setting.
### Canvas:SetLayer
Sets the frame layer for this frame. This can be any number. Frames are drawn in ascending order. If two frames have the same layer number, then the order of those frames is undefined, but guarantees no Z-fighting. Frames are always drawn after their parents.
 
    Frame:SetLayer(layer)   -- number
#### Parameters:
**layer** - The new layer for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Canvas:SetMouseMasking
Sets the frame's mouse masking mode.
 
    Element:SetMouseMasking(mask)   -- string
#### Parameters:
**mask** - The new mouse masking mode. "full" is the standard mode, and means that creating any Left, Right, or movement-related mouse event will cause the frame to accept and consume any event from any of those types. "limited" causes the frame to accept and consume only events for buttons that have been hooked, so that hooking "LeftDown" will still pass Right mouse events through the frame. Note that hooking any mouse event will still consume MouseMove/In/Out events.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Canvas:SetMouseoverUnit
Sets the unit that will be represented by this frame.
 
    Frame:SetMouseoverUnit(unit)   -- unit
    Frame:SetMouseoverUnit(unit)   -- nil
#### Parameters:
**unit** - The new mouseover unit. May be a unit ID or a unit specifier. Pass in nil to disable the mouseover effect.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### Canvas:SetParent
Sets the parent of this frame. Circular dependencies result in undefined behavior. If this frame's secure mode is "restricted", then its parent must also have a secure mode of "restricted".
 
    Frame:SetParent(parent)   -- Element
#### Parameters:
**parent** - The new parent for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Canvas:SetPoint
Pins a point on this frame to a location on another frame. This is a rather complex function and you should look at examples to see how to use it.
This function can take many different forms. In general, it looks like this: SetPoint(point_on_this_frame, target_frame, point_on_target_frame [, x_offset, y_offset]).
The first part is the point on this frame that will be attached. Usually, these are string identifiers. "TOPLEFT", "TOPCENTER", "TOPRIGHT", "CENTERLEFT", "CENTER", "CENTERRIGHT", "BOTTOMLEFT", "BOTTOMCENTER", "BOTTOMRIGHT". You may also use a string identifier that refers to a single axis - "TOP", "BOTTOM", "LEFT", "RIGHT", "CENTERX", "CENTERY". If you want more direct numeric control you can use number pairs. 0,0 is equivalent to "TOPLEFT", 1,1 is equivalent to "BOTTOMRIGHT", 0.5,nil is equivalent to "CENTERX".
The second part is the frame to attach to, as well as the point on that frame to attach to. The frame is simply passing in the frame table. The point is the same identifier or number pair as the first parameter.
Optionally, you may include an X or Y offset to the point.
This connection is permanent, and if the target frame moves, this frame will move along with it. 
Caveat: If the target is a frame set to the "restricted" SecureMode, and the client is currently in "secure" mode, then unexpected behavior may occur.
 
    Frame:SetPoint(...)   -- ...
#### Parameters:
**...** - This function's parameters are complicated. Read the above summary for details.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Canvas:SetSecureMode
Sets the frame's secure mode.
"normal" is the standard mode. It allows for most functionality to be used and does not restrict frames in combat. "restricted" allows for certain sensitive functions to be called, but disables a significant amount of functionality in combat. In order to change a frame to "restricted", its parent must already be "restricted". Note that a "restricted" frame can still have "normal" children.
If you are not planning to use any restricted functions, your frame should remain in normal mode.
At the moment, it is not possible to change from "restricted" back to "normal".
 
    Frame:SetSecureMode(secure)   -- string
#### Parameters:
**secure** - The new secure mode. Valid inputs are "normal" and "restricted".
### Canvas:SetShape
Sets the canvas shape. Must have at least one of "fill" or "stroke".
 
    Canvas:SetShape(path, fill, stroke)   -- nil, nil, nil
    Canvas:SetShape(path, fill, stroke)   -- table, table, nil
    Canvas:SetShape(path, fill, stroke)   -- table, nil, table
    Canvas:SetShape(path, fill, stroke)   -- table, table, table
#### Parameters:
**path** - A path. Consists of a table containing a series of tables, with consecutive integer keys starting from 1. Each internal table contains coordinates, either as an absolute value relative to the top-left corner of the frame, or Proportional to the frame size, as a number from 0 to 1. Giving both an absolute value and a proportional value for any coordinate is not allowed. Members:
				x/xProportional: x coordinate of this point. Required.
				y/yProportional: y coordinate of this point. Required.
				xControl/xControlProportional: x coordinate of the curve control handle. If this exists, yControl/yControlProportional must also exist.
				yControl/yControlProportional: y coordinate of the curve control handle. If this exists, xControl/xControlProportional must also exist.
**stroke** - A stroke style. Members:
				r: Red color value, betwen 0 and 1. Required.
				g: Green color value, betwen 0 and 1. Required.
				b: Blue color value, betwen 0 and 1. Required.
				a: Alpha color value, betwen 0 and 1. Defaults to 1.
				cap: Endcap behavior for non-loops. May be "none", "square", or "round". Defaults to "round".
				miter: Corner behavior. May be "miter", "round", or "bevel". Defaults to "miter".
				miterLimit: Determines how small the corner angle can be until the miter is chopped off. Defaults to 3. Available only if miter behavior is set to "miter".
				thickness: Line thickness. Required.
**fill** - A fill style. By default, "gradientLinear" spreads on the X axis, between 0 and 100. "gradientRadial" is centered around the local origin, with a radius of 100 units. "texture" starts at the origin and extends out to the texture size. Members:
				type: One of "solid", "gradientLinear", "gradientRadial", "texture". Required.
				r: Red color value, betwen 0 and 1. Available and required for "solid".
				g: Green color value, betwen 0 and 1. Available and required for "solid".
				b: Blue color value, betwen 0 and 1. Available and required for "solid".
				a: Alpha color value, betwen 0 and 1. Defaults to 1. Available for "solid".
				color: Gradient color values. A table containing a series of tables, with consecutive integer keys starting from 1. Each internal table contains color values and a gradient position. Available and required for "gradientLinear" and "gradientRadial". Members:
					r: Red color value, between 0 and 1. Required.
					g: Green color value, between 0 and 1. Required.
					b: Blue color value, between 0 and 1. Required.
					a: Alpha color value, between 0 and 1. Defaults to 1.
					position: Location within the gradient. Must be nondecreasing. Gradient automatically rescales to fit the largest position value seen.
				transform: An array containing the first two rows of a 3x3 transformation matrix. The third row is set to (0,0,1). Defaults to {1, 0, 0, 0, 1, 0}, representing the identity matrix. See Utility.Matrix.Create() for basic functionality. Available for "gradientLinear", "gradientRadial", and "texture".
				focus: Number between 0 and 1 controlling the center bias of radial gradients. Defaults to 0. Available for "gradientRadial".
				wrap: Control the bounds behavior of textures. Must be either "clamp" or "repeat". Defaults to "clamp". Available for "texture".
				smooth: Boolean controlling texture smoothing behavior. Defaults to true. Available for "texture".
				source: Source addon ID of the texture to be rendered. See Texture:SetTexture() for details. Available and required for "texture".
				texture: Texture identifier of the texture to be rendered. See Texture:SetTexture() for details. Available and required for "texture".
### Canvas:SetStrata
Sets the strata for this frame.
 
    Frame:SetStrata(strata)   -- string
#### Parameters:
**strata** - The item's new strata. Must be one of the elements returned by :GetStrataList().
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Canvas:SetVisible
Sets the frame's visibility flag. If set to false, then this frame and all its children will not be rendered or accept mouse input.
 
    Element:SetVisible(visible)   -- boolean
#### Parameters:
**visible** - The new visibility flag.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Canvas:SetWidth
Sets the width of this frame. Undefined results if the frame already has two pinned X coordinates.
 
    Frame:SetWidth(width)   -- number
#### Parameters:
**width** - The new width of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Frame:ClearAll
Clear all set points and sizes from this frame.
 
    Frame:ClearAll()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Frame:ClearHeight
Clear a set height from this frame.
 
    Frame:ClearHeight()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Frame:ClearPoint
Clear a set point from this frame.
 
    Frame:ClearPoint(coordinate)   -- string
    Frame:ClearPoint(x, y)   -- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point.
**x** - X coordinate of the point.
**coordinate** - Named coordinate.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Frame:ClearWidth
Clear a set width from this frame.
 
    Frame:ClearWidth()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Frame:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Frame:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
### Frame:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### Frame:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### Frame:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### Frame:GetAlpha
Gets the alpha multiplier of this frame.
 
    alpha = Element:GetAlpha()   -- number <- void
#### Return Values:
**alpha** - The alpha multiplier of this frame. 1 is fully opaque, 0 is fully transparent. This does not include multiplied alphas from this frame's parent - it's the exact value passed to SetAlpha.
### Frame:GetBackgroundColor
Retrieves the background color of this frame.
 
    r, g, b, a = Element:GetBackgroundColor()   -- number, number, number, number <- void
#### Return Values:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.
**g** - Green.
### Frame:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### Frame:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### Frame:GetChildren
Returns a table containing all of this element's children.
 
    children = Element:GetChildren()   -- table <- void
#### Return Values:
**children** - A table containing this element's children.
### Frame:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### Frame:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### Frame:GetKeyFocus
Gets the key focus status.
 
    focus = Element:GetKeyFocus()   -- boolean <- void
#### Return Values:
**focus** - Whether this frame is the current key focus.
### Frame:GetLayer
Gets the frame's layer order.
 
    layer = Frame:GetLayer()   -- number <- void
#### Return Values:
**layer** - The render layer of this frame. See Frame:SetLayer for details.
### Frame:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### Frame:GetMouseMasking
Get the current mouse masking mode. See SetMouseMasking for details.
 
    mask = Element:GetMouseMasking()   -- string <- void
#### Return Values:
**mask** - The current mouse masking mode.
### Frame:GetMouseoverUnit
Gets the unit that is being represented by this frame.
 
    unit = Frame:GetMouseoverUnit()   -- string <- void
#### Return Values:
**unit** - The current mouseover unit. May be nil if there is no mouseover unit.
### Frame:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
m] has successfully crafted [Carmintium Lariat]!
### Frame:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### Frame:GetParent
Gets the parent of this frame.
 
    parent = Frame:GetParent()   -- Element <- void
#### Return Values:
**parent** - The parent element of this frame.
### Frame:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### Frame:GetSecureMode
Get the current secure mode. See SetSecureMode for details.
 
    secure = Frame:GetSecureMode()   -- string <- void
#### Return Values:
**secure** - The current secure mode.
### Frame:GetStrata
Gets the frame's strata. The strata determines render order on a coarser level than Layer does, as well as determining how far an element is brought to the front when clicked on.
 
    strata = Frame:GetStrata()   -- string <- void
#### Return Values:
**strata** - The item's current strata.
### Frame:GetStrataList
Gets a list of valid stratas for this frame.
 
    stratas = Frame:GetStrataList()   -- table <- void
#### Return Values:
**stratas** - An array of valid stratas, in order.
### Frame:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### Frame:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### Frame:GetVisible
Gets the visibility flag for this frame.
 
    visible = Element:GetVisible()   -- boolean <- void
#### Return Values:
**visible** - This frame's visibility flag, as set by SetVisible. Does not check the visibility flags of the frame's parents.
### Frame:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### Frame:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### Frame:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### Frame:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### Frame:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### Frame:SetAllPoints
Pins all the edges of this frame to the edges of a different frame. If no target is given, defaults to this frame's parent.
 
    Frame:SetAllPoints()   -- void
    Frame:SetAllPoints(target)   -- Layout
#### Parameters:
**target** - Target Layout to pin this frame's edges to.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Frame:SetAlpha
Sets the alpha transparency multiplier for this frame and its children.
 
    Element:SetAlpha(alpha)   -- number
#### Parameters:
**alpha** - The new alpha multiplier. 1 is fully opaque, 0 is fully transparent.
### Frame:SetBackgroundColor
Sets the background color of this frame.
 
    Element:SetBackgroundColor(r, g, b)   -- number, number, number
    Element:SetBackgroundColor(r, g, b, a)   -- number, number, number, number
#### Parameters:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.
**g** - Green.
### Frame:SetHeight
Sets the height of this frame. Undefined results if the frame already has two pinned Y coordinates.
 
    Frame:SetHeight(height)   -- number
#### Parameters:
**height** - The new height of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Frame:SetKeyFocus
Sets the key focus status. Note that only one frame can be the key focus at a time. Focusing on another frame will automatically unset the current focus.
 
    Element:SetKeyFocus(focus)   -- boolean
#### Parameters:
**focus** - The new key focus setting.
### Frame:SetLayer
Sets the frame layer for this frame. This can be any number. Frames are drawn in ascending order. If two frames have the same layer number, then the order of those frames is undefined, but guarantees no Z-fighting. Frames are always drawn after their parents.
 
    Frame:SetLayer(layer)   -- number
#### Parameters:
**layer** - The new layer for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Frame:SetMouseMasking
Sets the frame's mouse masking mode.
 
    Element:SetMouseMasking(mask)   -- string
#### Parameters:
**mask** - The new mouse masking mode. "full" is the standard mode, and means that creating any Left, Right, or movement-related mouse event will cause the frame to accept and consume any event from any of those types. "limited" causes the frame to accept and consume only events for buttons that have been hooked, so that hooking "LeftDown" will still pass Right mouse events through the frame. Note that hooking any mouse event will still consume MouseMove/In/Out events.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Frame:SetMouseoverUnit
Sets the unit that will be represented by this frame.
 
    Frame:SetMouseoverUnit(unit)   -- unit
    Frame:SetMouseoverUnit(unit)   -- nil
#### Parameters:
**unit** - The new mouseover unit. May be a unit ID or a unit specifier. Pass in nil to disable the mouseover effect.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### Frame:SetParent
Sets the parent of this frame. Circular dependencies result in undefined behavior. If this frame's secure mode is "restricted", then its parent must also have a secure mode of "restricted".
 
    Frame:SetParent(parent)   -- Element
#### Parameters:
**parent** - The new parent for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Frame:SetPoint
Pins a point on this frame to a location on another frame. This is a rather complex function and you should look at examples to see how to use it.
This function can take many different forms. In general, it looks like this: SetPoint(point_on_this_frame, target_frame, point_on_target_frame [, x_offset, y_offset]).
The first part is the point on this frame that will be attached. Usually, these are string identifiers. "TOPLEFT", "TOPCENTER", "TOPRIGHT", "CENTERLEFT", "CENTER", "CENTERRIGHT", "BOTTOMLEFT", "BOTTOMCENTER", "BOTTOMRIGHT". You may also use a string identifier that refers to a single axis - "TOP", "BOTTOM", "LEFT", "RIGHT", "CENTERX", "CENTERY". If you want more direct numeric control you can use number pairs. 0,0 is equivalent to "TOPLEFT", 1,1 is equivalent to "BOTTOMRIGHT", 0.5,nil is equivalent to "CENTERX".
The second part is the frame to attach to, as well as the point on that frame to attach to. The frame is simply passing in the frame table. The point is the same identifier or number pair as the first parameter.
Optionally, you may include an X or Y offset to the point.
This connection is permanent, and if the target frame moves, this frame will move along with it. 
Caveat: If the target is a frame set to the "restricted" SecureMode, and the client is currently in "secure" mode, then unexpected behavior may occur.
 
    Frame:SetPoint(...)   -- ...
#### Parameters:
**...** - This function's parameters are complicated. Read the above summary for details.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Frame:SetSecureMode
Sets the frame's secure mode.
"normal" is the standard mode. It allows for most functionality to be used and does not restrict frames in combat. "restricted" allows for certain sensitive functions to be called, but disables a significant amount of functionality in combat. In order to change a frame to "restricted", its parent must already be "restricted". Note that a "restricted" frame can still have "normal" children.
If you are not planning to use any restricted functions, your frame should remain in normal mode.
At the moment, it is not possible to change from "restricted" back to "normal".
 
    Frame:SetSecureMode(secure)   -- string
#### Parameters:
**secure** - The new secure mode. Valid inputs are "normal" and "restricted".
### Frame:SetStrata
Sets the strata for this frame.
 
    Frame:SetStrata(strata)   -- string
#### Parameters:
**strata** - The item's new strata. Must be one of the elements returned by :GetStrataList().
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Frame:SetVisible
Sets the frame's visibility flag. If set to false, then this frame and all its children will not be rendered or accept mouse input.
 
    Element:SetVisible(visible)   -- boolean
#### Parameters:
**visible** - The new visibility flag.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Frame:SetWidth
Sets the width of this frame. Undefined results if the frame already has two pinned X coordinates.
 
    Frame:SetWidth(width)   -- number
#### Parameters:
**width** - The new width of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Layout:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Layout:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
### Layout:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### Layout:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### Layout:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### Layout:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### Layout:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### Layout:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### Layout:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### Layout:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### Layout:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
### Layout:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### Layout:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### Layout:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### Layout:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### Layout:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### Layout:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### Layout:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### Layout:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### Layout:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### Mask:ClearAll
Clear all set points and sizes from this frame.
 
    Frame:ClearAll()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Mask:ClearHeight
Clear a set height from this frame.
 
    Frame:ClearHeight()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Mask:ClearPoint
Clear a set point from this frame.
 
    Frame:ClearPoint(coordinate)   -- string
    Frame:ClearPoint(x, y)   -- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point.
**x** - X coordinate of the point.
**coordinate** - Named coordinate.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Mask:ClearWidth
Clear a set width from this frame.
 
    Frame:ClearWidth()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Mask:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Mask:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
### Mask:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### Mask:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### Mask:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### Mask:GetAlpha
Gets the alpha multiplier of this frame.
 
    alpha = Element:GetAlpha()   -- number <- void
#### Return Values:
**alpha** - The alpha multiplier of this frame. 1 is fully opaque, 0 is fully transparent. This does not include multiplied alphas from this frame's parent - it's the exact value passed to SetAlpha.
### Mask:GetBackgroundColor
Retrieves the background color of this frame.
 
    r, g, b, a = Element:GetBackgroundColor()   -- number, number, number, number <- void
#### Return Values:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.
**g** - Green.
### Mask:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### Mask:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### Mask:GetChildren
Returns a table containing all of this element's children.
 
    children = Element:GetChildren()   -- table <- void
#### Return Values:
**children** - A table containing this element's children.
### Mask:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### Mask:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### Mask:GetKeyFocus
Gets the key focus status.
 
    focus = Element:GetKeyFocus()   -- boolean <- void
#### Return Values:
**focus** - Whether this frame is the current key focus.
### Mask:GetLayer
Gets the frame's layer order.
 
    layer = Frame:GetLayer()   -- number <- void
#### Return Values:
**layer** - The render layer of this frame. See Frame:SetLayer for details.
### Mask:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### Mask:GetMouseMasking
Get the current mouse masking mode. See SetMouseMasking for details.
 
    mask = Element:GetMouseMasking()   -- string <- void
#### Return Values:
**mask** - The current mouse masking mode.
### Mask:GetMouseoverUnit
Gets the unit that is being represented by this frame.
 
    unit = Frame:GetMouseoverUnit()   -- string <- void
#### Return Values:
**unit** - The current mouseover unit. May be nil if there is no mouseover unit.
### Mask:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
### Mask:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### Mask:GetParent
Gets the parent of this frame.
 
    parent = Frame:GetParent()   -- Element <- void
#### Return Values:
**parent** - The parent element of this frame.
### Mask:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### Mask:GetSecureMode
Get the current secure mode. See SetSecureMode for details.
 
    secure = Frame:GetSecureMode()   -- string <- void
#### Return Values:
**secure** - The current secure mode.
### Mask:GetShape
Returns the current mask shape.
 
    shape = Mask:GetShape()   -- table <- void
#### Return Values:
**shape** - The current shape. See Canvas:SetShape() for details.
### Mask:GetStrata
Gets the frame's strata. The strata determines render order on a coarser level than Layer does, as well as determining how far an element is brought to the front when clicked on.
 
    strata = Frame:GetStrata()   -- string <- void
#### Return Values:
**strata** - The item's current strata.
### Mask:GetStrataList
Gets a list of valid stratas for this frame.
 
    stratas = Frame:GetStrataList()   -- table <- void
#### Return Values:
**stratas** - An array of valid stratas, in order.
### Mask:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### Mask:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### Mask:GetVisible
Gets the visibility flag for this frame.
 
    visible = Element:GetVisible()   -- boolean <- void
#### Return Values:
**visible** - This frame's visibility flag, as set by SetVisible. Does not check the visibility flags of the frame's parents.
### Mask:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### Mask:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### Mask:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### Mask:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### Mask:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### Mask:SetAllPoints
Pins all the edges of this frame to the edges of a different frame. If no target is given, defaults to this frame's parent.
 
    Frame:SetAllPoints()   -- void
    Frame:SetAllPoints(target)   -- Layout
#### Parameters:
**target** - Target Layout to pin this frame's edges to.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Mask:SetAlpha
Sets the alpha transparency multiplier for this frame and its children.
 
    Element:SetAlpha(alpha)   -- number
#### Parameters:
**alpha** - The new alpha multiplier. 1 is fully opaque, 0 is fully transparent.
### Mask:SetBackgroundColor
Sets the background color of this frame.
 
    Element:SetBackgroundColor(r, g, b)   -- number, number, number
    Element:SetBackgroundColor(r, g, b, a)   -- number, number, number, number
#### Parameters:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.
**g** - Green.
### Mask:SetHeight
Sets the height of this frame. Undefined results if the frame already has two pinned Y coordinates.
 
    Frame:SetHeight(height)   -- number
#### Parameters:
**height** - The new height of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Mask:SetKeyFocus
Sets the key focus status. Note that only one frame can be the key focus at a time. Focusing on another frame will automatically unset the current focus.
 
    Element:SetKeyFocus(focus)   -- boolean
#### Parameters:
**focus** - The new key focus setting.
### Mask:SetLayer
Sets the frame layer for this frame. This can be any number. Frames are drawn in ascending order. If two frames have the same layer number, then the order of those frames is undefined, but guarantees no Z-fighting. Frames are always drawn after their parents.
 
    Frame:SetLayer(layer)   -- number
#### Parameters:
**layer** - The new layer for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Mask:SetMouseMasking
Sets the frame's mouse masking mode.
 
    Element:SetMouseMasking(mask)   -- string
#### Parameters:
**mask** - The new mouse masking mode. "full" is the standard mode, and means that creating any Left, Right, or movement-related mouse event will cause the frame to accept and consume any event from any of those types. "limited" causes the frame to accept and consume only events for buttons that have been hooked, so that hooking "LeftDown" will still pass Right mouse events through the frame. Note that hooking any mouse event will still consume MouseMove/In/Out events.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Mask:SetMouseoverUnit
Sets the unit that will be represented by this frame.
 
    Frame:SetMouseoverUnit(unit)   -- unit
    Frame:SetMouseoverUnit(unit)   -- nil
#### Parameters:
**unit** - The new mouseover unit. May be a unit ID or a unit specifier. Pass in nil to disable the mouseover effect.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### Mask:SetParent
Sets the parent of this frame. Circular dependencies result in undefined behavior. If this frame's secure mode is "restricted", then its parent must also have a secure mode of "restricted".
 
    Frame:SetParent(parent)   -- Element
#### Parameters:
**parent** - The new parent for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Mask:SetPoint
Pins a point on this frame to a location on another frame. This is a rather complex function and you should look at examples to see how to use it.
This function can take many different forms. In general, it looks like this: SetPoint(point_on_this_frame, target_frame, point_on_target_frame [, x_offset, y_offset]).
The first part is the point on this frame that will be attached. Usually, these are string identifiers. "TOPLEFT", "TOPCENTER", "TOPRIGHT", "CENTERLEFT", "CENTER", "CENTERRIGHT", "BOTTOMLEFT", "BOTTOMCENTER", "BOTTOMRIGHT". You may also use a string identifier that refers to a single axis - "TOP", "BOTTOM", "LEFT", "RIGHT", "CENTERX", "CENTERY". If you want more direct numeric control you can use number pairs. 0,0 is equivalent to "TOPLEFT", 1,1 is equivalent to "BOTTOMRIGHT", 0.5,nil is equivalent to "CENTERX".
The second part is the frame to attach to, as well as the point on that frame to attach to. The frame is simply passing in the frame table. The point is the same identifier or number pair as the first parameter.
Optionally, you may include an X or Y offset to the point.
This connection is permanent, and if the target frame moves, this frame will move along with it. 
Caveat: If the target is a frame set to the "restricted" SecureMode, and the client is currently in "secure" mode, then unexpected behavior may occur.
 
    Frame:SetPoint(...)   -- ...
#### Parameters:
**...** - This function's parameters are complicated. Read the above summary for details.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Mask:SetSecureMode
Sets the frame's secure mode.
"normal" is the standard mode. It allows for most functionality to be used and does not restrict frames in combat. "restricted" allows for certain sensitive functions to be called, but disables a significant amount of functionality in combat. In order to change a frame to "restricted", its parent must already be "restricted". Note that a "restricted" frame can still have "normal" children.
If you are not planning to use any restricted functions, your frame should remain in normal mode.
At the moment, it is not possible to change from "restricted" back to "normal".
 
    Frame:SetSecureMode(secure)   -- string
#### Parameters:
**secure** - The new secure mode. Valid inputs are "normal" and "restricted".
### Mask:SetShape
Sets the mask shape. Note that masks with custom shapes will not cull mouse events according to the custom shape.
 
    Mask:SetShape(path)   -- nil
    Mask:SetShape(path)   -- table
#### Parameters:
**path** - A path. Consists of a table containing a series of tables, with consecutive integer keys starting from 1. Each internal table contains coordinates, either as an absolute value relative to the top-left corner of the frame, or Proportional to the frame size, as a number from 0 to 1. Giving both an absolute value and a proportional value for any coordinate is not allowed. Members:
				x/xProportional: x coordinate of this point. Required.
				y/yProportional: y coordinate of this point. Required.
				xControl/xControlProportional: x coordinate of the curve control handle. If this exists, yControl/yControlProportional must also exist.
				yControl/yControlProportional: y coordinate of the curve control handle. If this exists, xControl/xControlProportional must also exist.
### Mask:SetStrata
Sets the strata for this frame.
 
    Frame:SetStrata(strata)   -- string
#### Parameters:
**strata** - The item's new strata. Must be one of the elements returned by :GetStrataList().
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Mask:SetVisible
Sets the frame's visibility flag. If set to false, then this frame and all its children will not be rendered or accept mouse input.
 
    Element:SetVisible(visible)   -- boolean
#### Parameters:
**visible** - The new visibility flag.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Mask:SetWidth
Sets the width of this frame. Undefined results if the frame already has two pinned X coordinates.
 
    Frame:SetWidth(width)   -- number
#### Parameters:
**width** - The new width of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Native:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Native:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
### Native:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### Native:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### Native:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### Native:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### Native:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### Native:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### Native:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### Native:GetLayer
Gets the native item's layer order.
 
    layer = Native:GetLayer()   -- number <- void
#### Return Values:
**layer** - The render layer of this frame. See Frame:SetLayer for details.
### Native:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### Native:GetLoaded
Gets whether or not this native element is loaded and rendering.
 
    loaded = Native:GetLoaded()   -- boolean <- void
#### Return Values:
**loaded** - true if it is loaded.
### Native:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
### Native:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### Native:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### Native:GetSecureMode
Get the native element's secure mode. See Frame:SetSecureMode() for details.
 
    secure = Native:GetSecureMode()   -- string <- void
#### Return Values:
**secure** - The current secure mode.
### Native:GetStrata
Gets the native item's strata. The strata determines render order on a coarser level than Layer does, as well as determining how far an element is brought to the front when clicked on.
 
    strata = Native:GetStrata()   -- string <- void
#### Return Values:
**strata** - The item's current strata.
### Native:GetStrataList
Gets a list of valid stratas for this native element.
 
    stratas = Native:GetStrataList()   -- table <- void
#### Return Values:
**stratas** - An array of valid stratas, in order.
### Native:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### Native:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### Native:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### Native:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### Native:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### Native:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### Native:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### Native:SetLayer
Sets the frame layer for this native element. This can be any number. Frames are drawn in ascending order. If two frames have the same layer number, then the order of those frames is undefined, but guarantees no Z-fighting. Frames are always drawn after their parents.
 
    Native:SetLayer(layer)   -- number
#### Parameters:
**layer** - The new layer for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Native:SetStrata
Sets the strata for this native element.
 
    Native:SetStrata(layer)   -- string
#### Parameters:
**layer** - The new layer for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftButton:ClearAll
Clear all set points and sizes from this frame.
 
    Frame:ClearAll()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftButton:ClearHeight
Clear a set height from this frame.
 
    Frame:ClearHeight()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftButton:ClearPoint
Clear a set point from this frame.
 
    Frame:ClearPoint(coordinate)   -- string
    Frame:ClearPoint(x, y)   -- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point.
**x** - X coordinate of the point.
**coordinate** - Named coordinate.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftButton:ClearWidth
Clear a set width from this frame.
 
    Frame:ClearWidth()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftButton:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftButton:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftButton:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftButton:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### RiftButton:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### RiftButton:GetAlpha
Gets the alpha multiplier of this frame.
 
    alpha = Element:GetAlpha()   -- number <- void
#### Return Values:
**alpha** - The alpha multiplier of this frame. 1 is fully opaque, 0 is fully transparent. This does not include multiplied alphas from this frame's parent - it's the exact value passed to SetAlpha.
### RiftButton:GetBackgroundColor
Retrieves the background color of this frame.
 
    r, g, b, a = Element:GetBackgroundColor()   -- number, number, number, number <- void
#### Return Values:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.
**g** - Green.
### RiftButton:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### RiftButton:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### RiftButton:GetChildren
Returns a table containing all of this element's children.
 
    children = Element:GetChildren()   -- table <- void
#### Return Values:
**children** - A table containing this element's children.
### RiftButton:GetEnabled
Gets whether the button is enabled or grayed out.
 
    enabled = RiftButton:GetEnabled()   -- boolean <- void
#### Return Values:
**enabled** - The current enable state of this item.
### RiftButton:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### RiftButton:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### RiftButton:GetKeyFocus
Gets the key focus status.
 
    focus = Element:GetKeyFocus()   -- boolean <- void
#### Return Values:
**focus** - Whether this frame is the current key focus.
### RiftButton:GetLayer
Gets the frame's layer order.
 
    layer = Frame:GetLayer()   -- number <- void
#### Return Values:
**layer** - The render layer of this frame. See Frame:SetLayer for details.
### RiftButton:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### RiftButton:GetMouseMasking
Get the current mouse masking mode. See SetMouseMasking for details.
 
    mask = Element:GetMouseMasking()   -- string <- void
#### Return Values:
**mask** - The current mouse masking mode.
### RiftButton:GetMouseoverUnit
Gets the unit that is being represented by this frame.
 
    unit = Frame:GetMouseoverUnit()   -- string <- void
#### Return Values:
**unit** - The current mouseover unit. May be nil if there is no mouseover unit.
### RiftButton:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
### RiftButton:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### RiftButton:GetParent
Gets the parent of this frame.
 
    parent = Frame:GetParent()   -- Element <- void
#### Return Values:
**parent** - The parent element of this frame.
### RiftButton:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### RiftButton:GetSecureMode
Get the current secure mode. See SetSecureMode for details.
 
    secure = Frame:GetSecureMode()   -- string <- void
#### Return Values:
**secure** - The current secure mode.
### RiftButton:GetSkin
Gets the current skin of this item.
 
    skin = RiftButton:GetSkin()   -- string <- void
#### Return Values:
**skin** - The current skin. Possible results include "default" and "close".
### RiftButton:GetStrata
Gets the frame's strata. The strata determines render order on a coarser level than Layer does, as well as determining how far an element is brought to the front when clicked on.
 
    strata = Frame:GetStrata()   -- string <- void
#### Return Values:
**strata** - The item's current strata.
### RiftButton:GetStrataList
Gets a list of valid stratas for this frame.
 
    stratas = Frame:GetStrataList()   -- table <- void
#### Return Values:
**stratas** - An array of valid stratas, in order.
### RiftButton:GetText
Gets this button's text.
 
    text = RiftButton:GetText()   -- string <- void
#### Return Values:
**text** - The current text of the element.
### RiftButton:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### RiftButton:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### RiftButton:GetVisible
Gets the visibility flag for this frame.
 
    visible = Element:GetVisible()   -- boolean <- void
#### Return Values:
**visible** - This frame's visibility flag, as set by SetVisible. Does not check the visibility flags of the frame's parents.
### RiftButton:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### RiftButton:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### RiftButton:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### RiftButton:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### RiftButton:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### RiftButton:SetAllPoints
Pins all the edges of this frame to the edges of a different frame. If no target is given, defaults to this frame's parent.
 
    Frame:SetAllPoints()   -- void
    Frame:SetAllPoints(target)   -- Layout
#### Parameters:
**target** - Target Layout to pin this frame's edges to.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftButton:SetAlpha
Sets the alpha transparency multiplier for this frame and its children.
 
    Element:SetAlpha(alpha)   -- number
#### Parameters:
**alpha** - The new alpha multiplier. 1 is fully opaque, 0 is fully transparent.
### RiftButton:SetBackgroundColor
Sets the background color of this frame.
 
    Element:SetBackgroundColor(r, g, b)   -- number, number, number
    Element:SetBackgroundColor(r, g, b, a)   -- number, number, number, number
#### Parameters:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.
**g** - Green.
### RiftButton:SetEnabled
Sets whether the button is enabled or grayed out.
 
    RiftButton:SetEnabled(enabled)   -- boolean
#### Parameters:
**enabled** - The new enable state of this item.
### RiftButton:SetHeight
Sets the height of this frame. Undefined results if the frame already has two pinned Y coordinates.
 
    Frame:SetHeight(height)   -- number
#### Parameters:
**height** - The new height of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftButton:SetKeyFocus
Sets the key focus status. Note that only one frame can be the key focus at a time. Focusing on another frame will automatically unset the current focus.
 
    Element:SetKeyFocus(focus)   -- boolean
#### Parameters:
**focus** - The new key focus setting.
### RiftButton:SetLayer
Sets the frame layer for this frame. This can be any number. Frames are drawn in ascending order. If two frames have the same layer number, then the order of those frames is undefined, but guarantees no Z-fighting. Frames are always drawn after their parents.
 
    Frame:SetLayer(layer)   -- number
#### Parameters:
**layer** - The new layer for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftButton:SetMouseMasking
Sets the frame's mouse masking mode.
 
    Element:SetMouseMasking(mask)   -- string
#### Parameters:
**mask** - The new mouse masking mode. "full" is the standard mode, and means that creating any Left, Right, or movement-related mouse event will cause the frame to accept and consume any event from any of those types. "limited" causes the frame to accept and consume only events for buttons that have been hooked, so that hooking "LeftDown" will still pass Right mouse events through the frame. Note that hooking any mouse event will still consume MouseMove/In/Out events.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftButton:SetMouseoverUnit
Sets the unit that will be represented by this frame.
 
    Frame:SetMouseoverUnit(unit)   -- unit
    Frame:SetMouseoverUnit(unit)   -- nil
#### Parameters:
**unit** - The new mouseover unit. May be a unit ID or a unit specifier. Pass in nil to disable the mouseover effect.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### RiftButton:SetParent
Sets the parent of this frame. Circular dependencies result in undefined behavior. If this frame's secure mode is "restricted", then its parent must also have a secure mode of "restricted".
 
    Frame:SetParent(parent)   -- Element
#### Parameters:
**parent** - The new parent for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftButton:SetPoint
Pins a point on this frame to a location on another frame. This is a rather complex function and you should look at examples to see how to use it.
This function can take many different forms. In general, it looks like this: SetPoint(point_on_this_frame, target_frame, point_on_target_frame [, x_offset, y_offset]).
The first part is the point on this frame that will be attached. Usually, these are string identifiers. "TOPLEFT", "TOPCENTER", "TOPRIGHT", "CENTERLEFT", "CENTER", "CENTERRIGHT", "BOTTOMLEFT", "BOTTOMCENTER", "BOTTOMRIGHT". You may also use a string identifier that refers to a single axis - "TOP", "BOTTOM", "LEFT", "RIGHT", "CENTERX", "CENTERY". If you want more direct numeric control you can use number pairs. 0,0 is equivalent to "TOPLEFT", 1,1 is equivalent to "BOTTOMRIGHT", 0.5,nil is equivalent to "CENTERX".
The second part is the frame to attach to, as well as the point on that frame to attach to. The frame is simply passing in the frame table. The point is the same identifier or number pair as the first parameter.
Optionally, you may include an X or Y offset to the point.
This connection is permanent, and if the target frame moves, this frame will move along with it. 
Caveat: If the target is a frame set to the "restricted" SecureMode, and the client is currently in "secure" mode, then unexpected behavior may occur.
 
    Frame:SetPoint(...)   -- ...
#### Parameters:
**...** - This function's parameters are complicated. Read the above summary for details.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftButton:SetSecureMode
Sets the frame's secure mode.
"normal" is the standard mode. It allows for most functionality to be used and does not restrict frames in combat. "restricted" allows for certain sensitive functions to be called, but disables a significant amount of functionality in combat. In order to change a frame to "restricted", its parent must already be "restricted". Note that a "restricted" frame can still have "normal" children.
If you are not planning to use any restricted functions, your frame should remain in normal mode.
At the moment, it is not possible to change from "restricted" back to "normal".
 
    Frame:SetSecureMode(secure)   -- string
#### Parameters:
**secure** - The new secure mode. Valid inputs are "normal" and "restricted".
### RiftButton:SetSkin
Sets the current skin of this item.
 
    RiftButton:SetSkin(skin)   -- string
#### Parameters:
**skin** - The new skin. Possible values are "default" and "close".
### RiftButton:SetStrata
Sets the strata for this frame.
 
    Frame:SetStrata(strata)   -- string
#### Parameters:
**strata** - The item's new strata. Must be one of the elements returned by :GetStrataList().
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftButton:SetText
Sets this button's text.
 
    RiftButton:SetText(text)   -- string
#### Parameters:
**text** - The new text for the element.
### RiftButton:SetVisible
Sets the frame's visibility flag. If set to false, then this frame and all its children will not be rendered or accept mouse input.
 
    Element:SetVisible(visible)   -- boolean
#### Parameters:
**visible** - The new visibility flag.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftButton:SetWidth
Sets the width of this frame. Undefined results if the frame already has two pinned X coordinates.
 
    Frame:SetWidth(width)   -- number
#### Parameters:
**width** - The new width of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftCheckbox:ClearAll
Clear all set points and sizes from this frame.
 
    Frame:ClearAll()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftCheckbox:ClearHeight
Clear a set height from this frame.
 
    Frame:ClearHeight()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftCheckbox:ClearPoint
Clear a set point from this frame.
 
    Frame:ClearPoint(coordinate)   -- string
    Frame:ClearPoint(x, y)   -- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point.
**x** - X coordinate of the point.
**coordinate** - Named coordinate.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftCheckbox:ClearWidth
Clear a set width from this frame.
 
    Frame:ClearWidth()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftCheckbox:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftCheckbox:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftCheckbox:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftCheckbox:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### RiftCheckbox:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### RiftCheckbox:GetAlpha
Gets the alpha multiplier of this frame.
 
    alpha = Element:GetAlpha()   -- number <- void
#### Return Values:
**alpha** - The alpha multiplier of this frame. 1 is fully opaque, 0 is fully transparent. This does not include multiplied alphas from this frame's parent - it's the exact value passed to SetAlpha.
### RiftCheckbox:GetBackgroundColor
Retrieves the background color of this frame.
 
    r, g, b, a = Element:GetBackgroundColor()   -- number, number, number, number <- void
#### Return Values:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.
**g** - Green.
### RiftCheckbox:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### RiftCheckbox:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### RiftCheckbox:GetChecked
Gets whether the button is checked or not.
 
    checked = RiftCheckbox:GetChecked()   -- boolean <- void
#### Return Values:
**checked** - The current checked state of this item.
### RiftCheckbox:GetChildren
Returns a table containing all of this element's children.
 
    children = Element:GetChildren()   -- table <- void
#### Return Values:
**children** - A table containing this element's children.
### RiftCheckbox:GetEnabled
Gets whether the checkbox is enabled or grayed out.
 
    enabled = RiftCheckbox:GetEnabled()   -- boolean <- void
#### Return Values:
**enabled** - The current enable state of this item.
### RiftCheckbox:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### RiftCheckbox:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### RiftCheckbox:GetKeyFocus
Gets the key focus status.
 
    focus = Element:GetKeyFocus()   -- boolean <- void
#### Return Values:
**focus** - Whether this frame is the current key focus.
### RiftCheckbox:GetLayer
Gets the frame's layer order.
 
    layer = Frame:GetLayer()   -- number <- void
#### Return Values:
**layer** - The render layer of this frame. See Frame:SetLayer for details.
### RiftCheckbox:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### RiftCheckbox:GetMouseMasking
Get the current mouse masking mode. See SetMouseMasking for details.
 
    mask = Element:GetMouseMasking()   -- string <- void
#### Return Values:
**mask** - The current mouse masking mode.
### RiftCheckbox:GetMouseoverUnit
Gets the unit that is being represented by this frame.
 
    unit = Frame:GetMouseoverUnit()   -- string <- void
#### Return Values:
**unit** - The current mouseover unit. May be nil if there is no mouseover unit.
### RiftCheckbox:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
### RiftCheckbox:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### RiftCheckbox:GetParent
Gets the parent of this frame.
 
    parent = Frame:GetParent()   -- Element <- void
#### Return Values:
**parent** - The parent element of this frame.
### RiftCheckbox:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### RiftCheckbox:GetSecureMode
Get the current secure mode. See SetSecureMode for details.
 
    secure = Frame:GetSecureMode()   -- string <- void
#### Return Values:
**secure** - The current secure mode.
### RiftCheckbox:GetStrata
Gets the frame's strata. The strata determines render order on a coarser level than Layer does, as well as determining how far an element is brought to the front when clicked on.
 
    strata = Frame:GetStrata()   -- string <- void
#### Return Values:
**strata** - The item's current strata.
### RiftCheckbox:GetStrataList
Gets a list of valid stratas for this frame.
 
    stratas = Frame:GetStrataList()   -- table <- void
#### Return Values:
**stratas** - An array of valid stratas, in order.
### RiftCheckbox:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### RiftCheckbox:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### RiftCheckbox:GetVisible
Gets the visibility flag for this frame.
 
    visible = Element:GetVisible()   -- boolean <- void
#### Return Values:
**visible** - This frame's visibility flag, as set by SetVisible. Does not check the visibility flags of the frame's parents.
### RiftCheckbox:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### RiftCheckbox:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### RiftCheckbox:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### RiftCheckbox:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### RiftCheckbox:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### RiftCheckbox:SetAllPoints
Pins all the edges of this frame to the edges of a different frame. If no target is given, defaults to this frame's parent.
 
    Frame:SetAllPoints()   -- void
    Frame:SetAllPoints(target)   -- Layout
#### Parameters:
**target** - Target Layout to pin this frame's edges to.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftCheckbox:SetAlpha
Sets the alpha transparency multiplier for this frame and its children.
 
    Element:SetAlpha(alpha)   -- number
#### Parameters:
**alpha** - The new alpha multiplier. 1 is fully opaque, 0 is fully transparent.
### RiftCheckbox:SetBackgroundColor
Sets the background color of this frame.
 
    Element:SetBackgroundColor(r, g, b)   -- number, number, number
    Element:SetBackgroundColor(r, g, b, a)   -- number, number, number, number
#### Parameters:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.
**g** - Green.
### RiftCheckbox:SetChecked
Sets whether the checkbox is checked or not.
 
    RiftCheckbox:SetChecked(checked)   -- boolean
#### Parameters:
**checked** - The new checked state of this item.
### RiftCheckbox:SetEnabled
Sets whether the checkbox is enabled or grayed out.
 
    RiftCheckbox:SetEnabled(enabled)   -- boolean
#### Parameters:
**enabled** - The new enable state of this item.
### RiftCheckbox:SetHeight
Sets the height of this frame. Undefined results if the frame already has two pinned Y coordinates.
 
    Frame:SetHeight(height)   -- number
#### Parameters:
**height** - The new height of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftCheckbox:SetKeyFocus
Sets the key focus status. Note that only one frame can be the key focus at a time. Focusing on another frame will automatically unset the current focus.
 
    Element:SetKeyFocus(focus)   -- boolean
#### Parameters:
**focus** - The new key focus setting.
### RiftCheckbox:SetLayer
Sets the frame layer for this frame. This can be any number. Frames are drawn in ascending order. If two frames have the same layer number, then the order of those frames is undefined, but guarantees no Z-fighting. Frames are always drawn after their parents.
 
    Frame:SetLayer(layer)   -- number
#### Parameters:
**layer** - The new layer for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftCheckbox:SetMouseMasking
Sets the frame's mouse masking mode.
 
    Element:SetMouseMasking(mask)   -- string
#### Parameters:
**mask** - The new mouse masking mode. "full" is the standard mode, and means that creating any Left, Right, or movement-related mouse event will cause the frame to accept and consume any event from any of those types. "limited" causes the frame to accept and consume only events for buttons that have been hooked, so that hooking "LeftDown" will still pass Right mouse events through the frame. Note that hooking any mouse event will still consume MouseMove/In/Out events.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftCheckbox:SetMouseoverUnit
Sets the unit that will be represented by this frame.
 
    Frame:SetMouseoverUnit(unit)   -- unit
    Frame:SetMouseoverUnit(unit)   -- nil
#### Parameters:
**unit** - The new mouseover unit. May be a unit ID or a unit specifier. Pass in nil to disable the mouseover effect.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### RiftCheckbox:SetParent
Sets the parent of this frame. Circular dependencies result in undefined behavior. If this frame's secure mode is "restricted", then its parent must also have a secure mode of "restricted".
 
    Frame:SetParent(parent)   -- Element
#### Parameters:
**parent** - The new parent for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftCheckbox:SetPoint
Pins a point on this frame to a location on another frame. This is a rather complex function and you should look at examples to see how to use it.
This function can take many different forms. In general, it looks like this: SetPoint(point_on_this_frame, target_frame, point_on_target_frame [, x_offset, y_offset]).
The first part is the point on this frame that will be attached. Usually, these are string identifiers. "TOPLEFT", "TOPCENTER", "TOPRIGHT", "CENTERLEFT", "CENTER", "CENTERRIGHT", "BOTTOMLEFT", "BOTTOMCENTER", "BOTTOMRIGHT". You may also use a string identifier that refers to a single axis - "TOP", "BOTTOM", "LEFT", "RIGHT", "CENTERX", "CENTERY". If you want more direct numeric control you can use number pairs. 0,0 is equivalent to "TOPLEFT", 1,1 is equivalent to "BOTTOMRIGHT", 0.5,nil is equivalent to "CENTERX".
The second part is the frame to attach to, as well as the point on that frame to attach to. The frame is simply passing in the frame table. The point is the same identifier or number pair as the first parameter.
Optionally, you may include an X or Y offset to the point.
This connection is permanent, and if the target frame moves, this frame will move along with it. 
Caveat: If the target is a frame set to the "restricted" SecureMode, and the client is currently in "secure" mode, then unexpected behavior may occur.
 
    Frame:SetPoint(...)   -- ...
#### Parameters:
**...** - This function's parameters are complicated. Read the above summary for details.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftCheckbox:SetSecureMode
Sets the frame's secure mode.
"normal" is the standard mode. It allows for most functionality to be used and does not restrict frames in combat. "restricted" allows for certain sensitive functions to be called, but disables a significant amount of functionality in combat. In order to change a frame to "restricted", its parent must already be "restricted". Note that a "restricted" frame can still have "normal" children.
If you are not planning to use any restricted functions, your frame should remain in normal mode.
At the moment, it is not possible to change from "restricted" back to "normal".
 
    Frame:SetSecureMode(secure)   -- string
#### Parameters:
**secure** - The new secure mode. Valid inputs are "normal" and "restricted".
### RiftCheckbox:SetStrata
Sets the strata for this frame.
 
    Frame:SetStrata(strata)   -- string
#### Parameters:
**strata** - The item's new strata. Must be one of the elements returned by :GetStrataList().
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftCheckbox:SetVisible
Sets the frame's visibility flag. If set to false, then this frame and all its children will not be rendered or accept mouse input.
 
    Element:SetVisible(visible)   -- boolean
#### Parameters:
**visible** - The new visibility flag.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftCheckbox:SetWidth
Sets the width of this frame. Undefined results if the frame already has two pinned X coordinates.
 
    Frame:SetWidth(width)   -- number
#### Parameters:
**width** - The new width of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftScrollbar:ClearAll
Clear all set points and sizes from this frame.
 
    Frame:ClearAll()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftScrollbar:ClearHeight
Clear a set height from this frame.
 
    Frame:ClearHeight()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftScrollbar:ClearPoint
Clear a set point from this frame.
 
    Frame:ClearPoint(coordinate)   -- string
    Frame:ClearPoint(x, y)   -- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point.
**x** - X coordinate of the point.
**coordinate** - Named coordinate.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftScrollbar:ClearWidth
Clear a set width from this frame.
 
    Frame:ClearWidth()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftScrollbar:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftScrollbar:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftScrollbar:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftScrollbar:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### RiftScrollbar:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### RiftScrollbar:GetAlpha
Gets the alpha multiplier of this frame.
 
    alpha = Element:GetAlpha()   -- number <- void
#### Return Values:
**alpha** - The alpha multiplier of this frame. 1 is fully opaque, 0 is fully transparent. This does not include multiplied alphas from this frame's parent - it's the exact value passed to SetAlpha.
### RiftScrollbar:GetBackgroundColor
Retrieves the background color of this frame.
 
    r, g, b, a = Element:GetBackgroundColor()   -- number, number, number, number <- void
#### Return Values:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.
**g** - Green.
### RiftScrollbar:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### RiftScrollbar:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### RiftScrollbar:GetChildren
Returns a table containing all of this element's children.
 
    children = Element:GetChildren()   -- table <- void
#### Return Values:
**children** - A table containing this element's children.
### RiftScrollbar:GetEnabled
Gets whether the scrollbar is enabled or disabled.
 
    enabled = RiftScrollbar:GetEnabled()   -- boolean <- void
#### Return Values:
**enabled** - The current enable state of this item.
### RiftScrollbar:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### RiftScrollbar:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### RiftScrollbar:GetKeyFocus
Gets the key focus status.
 
    focus = Element:GetKeyFocus()   -- boolean <- void
#### Return Values:
**focus** - Whether this frame is the current key focus.
### RiftScrollbar:GetLayer
Gets the frame's layer order.
 
    layer = Frame:GetLayer()   -- number <- void
#### Return Values:
**layer** - The render layer of this frame. See Frame:SetLayer for details.
### RiftScrollbar:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### RiftScrollbar:GetMouseMasking
Get the current mouse masking mode. See SetMouseMasking for details.
 
    mask = Element:GetMouseMasking()   -- string <- void
#### Return Values:
**mask** - The current mouse masking mode.
### RiftScrollbar:GetMouseoverUnit
Gets the unit that is being represented by this frame.
 
    unit = Frame:GetMouseoverUnit()   -- string <- void
#### Return Values:
**unit** - The current mouseover unit. May be nil if there is no mouseover unit.
### RiftScrollbar:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
### RiftScrollbar:GetOrientation
Gets the current orientation of the scrollbar.
 
    orientation = RiftScrollbar:GetOrientation()   -- string <- void
#### Return Values:
**orientation** - The current orientation. Valid results include "vertical" and "horizontal".
### RiftScrollbar:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### RiftScrollbar:GetParent
Gets the parent of this frame.
 
    parent = Frame:GetParent()   -- Element <- void
#### Return Values:
**parent** - The parent element of this frame.
### RiftScrollbar:GetPosition
Returns the current position of the scrollbar.
 
    position = RiftScrollbar:GetPosition()   -- number <- void
#### Return Values:
**position** - The current position of this scrollbar.
### RiftScrollbar:GetRange
Returns the current range of the scrollbar.
 
    minimum, maximum = RiftScrollbar:GetRange()   -- number, number <- void
#### Return Values:
**maximum** - The maximum value for the position of this slider.
**minimum** - The minimum value for the position of this slider.
### RiftScrollbar:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### RiftScrollbar:GetSecureMode
Get the current secure mode. See SetSecureMode for details.
 
    secure = Frame:GetSecureMode()   -- string <- void
#### Return Values:
**secure** - The current secure mode.
### RiftScrollbar:GetStrata
Gets the frame's strata. The strata determines render order on a coarser level than Layer does, as well as determining how far an element is brought to the front when clicked on.
 
    strata = Frame:GetStrata()   -- string <- void
#### Return Values:
**strata** - The item's current strata.
### RiftScrollbar:GetStrataList
Gets a list of valid stratas for this frame.
 
    stratas = Frame:GetStrataList()   -- table <- void
#### Return Values:
**stratas** - An array of valid stratas, in order.
### RiftScrollbar:GetThickness
Returns the thickness of the scrollbar handle. Size is relative to the range.
 
    thickness = RiftScrollbar:GetThickness()   -- number <- void
#### Return Values:
**thickness** - The thickness of the handle.
### RiftScrollbar:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### RiftScrollbar:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### RiftScrollbar:GetVisible
Gets the visibility flag for this frame.
 
    visible = Element:GetVisible()   -- boolean <- void
#### Return Values:
**visible** - This frame's visibility flag, as set by SetVisible. Does not check the visibility flags of the frame's parents.
### RiftScrollbar:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### RiftScrollbar:Nudge
Modify the scrollbar position, clamped to the current bounds.
 
    RiftScrollbar:Nudge(offset)   -- number
#### Parameters:
**offset** - The amount to nudge.
### RiftScrollbar:NudgeDown
Shift the scrollbar down (i.e. away from zero) by a standard amount.
 
    RiftScrollbar:NudgeDown()   -- void
### RiftScrollbar:NudgeUp
Shift the scrollbar up (i.e. towards zero) by a standard amount.
 
    RiftScrollbar:NudgeUp()   -- void
### RiftScrollbar:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### RiftScrollbar:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### RiftScrollbar:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### RiftScrollbar:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### RiftScrollbar:SetAllPoints
Pins all the edges of this frame to the edges of a different frame. If no target is given, defaults to this frame's parent.
 
    Frame:SetAllPoints()   -- void
    Frame:SetAllPoints(target)   -- Layout
#### Parameters:
**target** - Target Layout to pin this frame's edges to.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftScrollbar:SetAlpha
Sets the alpha transparency multiplier for this frame and its children.
 
    Element:SetAlpha(alpha)   -- number
#### Parameters:
**alpha** - The new alpha multiplier. 1 is fully opaque, 0 is fully transparent.
### RiftScrollbar:SetBackgroundColor
Sets the background color of this frame.
 
    Element:SetBackgroundColor(r, g, b)   -- number, number, number
    Element:SetBackgroundColor(r, g, b, a)   -- number, number, number, number
#### Parameters:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.
**g** - Green.
### RiftScrollbar:SetEnabled
Sets whether the scrollbar is enabled or disabled.
 
    RiftScrollbar:SetEnabled(enabled)   -- boolean
#### Parameters:
**enabled** - The new enable state of this item.
### RiftScrollbar:SetHeight
Sets the height of this frame. Undefined results if the frame already has two pinned Y coordinates.
 
    Frame:SetHeight(height)   -- number
#### Parameters:
**height** - The new height of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftScrollbar:SetKeyFocus
Sets the key focus status. Note that only one frame can be the key focus at a time. Focusing on another frame will automatically unset the current focus.
 
    Element:SetKeyFocus(focus)   -- boolean
#### Parameters:
**focus** - The new key focus setting.
### RiftScrollbar:SetLayer
Sets the frame layer for this frame. This can be any number. Frames are drawn in ascending order. If two frames have the same layer number, then the order of those frames is undefined, but guarantees no Z-fighting. Frames are always drawn after their parents.
 
    Frame:SetLayer(layer)   -- number
#### Parameters:
**layer** - The new layer for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftScrollbar:SetMouseMasking
Sets the frame's mouse masking mode.
 
    Element:SetMouseMasking(mask)   -- string
#### Parameters:
**mask** - The new mouse masking mode. "full" is the standard mode, and means that creating any Left, Right, or movement-related mouse event will cause the frame to accept and consume any event from any of those types. "limited" causes the frame to accept and consume only events for buttons that have been hooked, so that hooking "LeftDown" will still pass Right mouse events through the frame. Note that hooking any mouse event will still consume MouseMove/In/Out events.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftScrollbar:SetMouseoverUnit
Sets the unit that will be represented by this frame.
 
    Frame:SetMouseoverUnit(unit)   -- unit
    Frame:SetMouseoverUnit(unit)   -- nil
#### Parameters:
**unit** - The new mouseover unit. May be a unit ID or a unit specifier. Pass in nil to disable the mouseover effect.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### RiftScrollbar:SetOrientation
Sets the orientation of the scrollbar.
 
    RiftScrollbar:SetOrientation(orientation)   -- string
#### Parameters:
**orientation** - The new orientation. Must be either "vertical" or "horizontal".
### RiftScrollbar:SetParent
Sets the parent of this frame. Circular dependencies result in undefined behavior. If this frame's secure mode is "restricted", then its parent must also have a secure mode of "restricted".
 
    Frame:SetParent(parent)   -- Element
#### Parameters:
**parent** - The new parent for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftScrollbar:SetPoint
Pins a point on this frame to a location on another frame. This is a rather complex function and you should look at examples to see how to use it.
This function can take many different forms. In general, it looks like this: SetPoint(point_on_this_frame, target_frame, point_on_target_frame [, x_offset, y_offset]).
The first part is the point on this frame that will be attached. Usually, these are string identifiers. "TOPLEFT", "TOPCENTER", "TOPRIGHT", "CENTERLEFT", "CENTER", "CENTERRIGHT", "BOTTOMLEFT", "BOTTOMCENTER", "BOTTOMRIGHT". You may also use a string identifier that refers to a single axis - "TOP", "BOTTOM", "LEFT", "RIGHT", "CENTERX", "CENTERY". If you want more direct numeric control you can use number pairs. 0,0 is equivalent to "TOPLEFT", 1,1 is equivalent to "BOTTOMRIGHT", 0.5,nil is equivalent to "CENTERX".
The second part is the frame to attach to, as well as the point on that frame to attach to. The frame is simply passing in the frame table. The point is the same identifier or number pair as the first parameter.
Optionally, you may include an X or Y offset to the point.
This connection is permanent, and if the target frame moves, this frame will move along with it. 
Caveat: If the target is a frame set to the "restricted" SecureMode, and the client is currently in "secure" mode, then unexpected behavior may occur.
 
    Frame:SetPoint(...)   -- ...
#### Parameters:
**...** - This function's parameters are complicated. Read the above summary for details.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftScrollbar:SetPosition
Changes the current position of the scrollbar.
 
    RiftScrollbar:SetPosition(position)   -- number
#### Parameters:
**position** - The new position of this scrollbar. Must be within the current range.
### RiftScrollbar:SetRange
Changes the current range of the scrollbar.
 
    RiftScrollbar:SetRange(minimum, maximum)   -- number, number
#### Parameters:
**maximum** - The new maximum value for the position of this slider. Must be an integer and larger than or equal to "minimum".
**minimum** - The new minimum value for the position of this slider. Must be an integer and smaller than or equal to "maximum".
### RiftScrollbar:SetSecureMode
Sets the frame's secure mode.
"normal" is the standard mode. It allows for most functionality to be used and does not restrict frames in combat. "restricted" allows for certain sensitive functions to be called, but disables a significant amount of functionality in combat. In order to change a frame to "restricted", its parent must already be "restricted". Note that a "restricted" frame can still have "normal" children.
If you are not planning to use any restricted functions, your frame should remain in normal mode.
At the moment, it is not possible to change from "restricted" back to "normal".
 
    Frame:SetSecureMode(secure)   -- string
#### Parameters:
**secure** - The new secure mode. Valid inputs are "normal" and "restricted".
### RiftScrollbar:SetStrata
Sets the strata for this frame.
 
    Frame:SetStrata(strata)   -- string
#### Parameters:
**strata** - The item's new strata. Must be one of the elements returned by :GetStrataList().
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftScrollbar:SetThickness
Sets the current thickness of the scrollbar handle. Size is relative to the range.
 
    RiftScrollbar:SetThickness(thickness)   -- number
#### Parameters:
**thickness** - The new thickness of the handle.
### RiftScrollbar:SetVisible
Sets the frame's visibility flag. If set to false, then this frame and all its children will not be rendered or accept mouse input.
 
    Element:SetVisible(visible)   -- boolean
#### Parameters:
**visible** - The new visibility flag.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftScrollbar:SetWidth
Sets the width of this frame. Undefined results if the frame already has two pinned X coordinates.
 
    Frame:SetWidth(width)   -- number
#### Parameters:
**width** - The new width of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftSlider:ClearAll
Clear all set points and sizes from this frame.
 
    Frame:ClearAll()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftSlider:ClearHeight
Clear a set height from this frame.
 
    Frame:ClearHeight()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftSlider:ClearPoint
Clear a set point from this frame.
 
    Frame:ClearPoint(coordinate)   -- string
    Frame:ClearPoint(x, y)   -- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point.
**x** - X coordinate of the point.
**coordinate** - Named coordinate.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftSlider:ClearWidth
Clear a set width from this frame.
 
    Frame:ClearWidth()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftSlider:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftSlider:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftSlider:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftSlider:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### RiftSlider:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### RiftSlider:GetAlpha
Gets the alpha multiplier of this frame.
 
    alpha = Element:GetAlpha()   -- number <- void
#### Return Values:
**alpha** - The alpha multiplier of this frame. 1 is fully opaque, 0 is fully transparent. This does not include multiplied alphas from this frame's parent - it's the exact value passed to SetAlpha.
### RiftSlider:GetBackgroundColor
Retrieves the background color of this frame.
 
    r, g, b, a = Element:GetBackgroundColor()   -- number, number, number, number <- void
#### Return Values:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.
**g** - Green.
### RiftSlider:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### RiftSlider:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### RiftSlider:GetChildren
Returns a table containing all of this element's children.
 
    children = Element:GetChildren()   -- table <- void
#### Return Values:
**children** - A table containing this element's children.
### RiftSlider:GetEnabled
Gets whether the slider is enabled or grayed out.
 
    enabled = RiftSlider:GetEnabled()   -- boolean <- void
#### Return Values:
**enabled** - The current enable state of this item.
### RiftSlider:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### RiftSlider:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### RiftSlider:GetKeyFocus
Gets the key focus status.
 
    focus = Element:GetKeyFocus()   -- boolean <- void
#### Return Values:
**focus** - Whether this frame is the current key focus.
### RiftSlider:GetLayer
Gets the frame's layer order.
 
    layer = Frame:GetLayer()   -- number <- void
#### Return Values:
**layer** - The render layer of this frame. See Frame:SetLayer for details.
### RiftSlider:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### RiftSlider:GetMouseMasking
Get the current mouse masking mode. See SetMouseMasking for details.
 
    mask = Element:GetMouseMasking()   -- string <- void
#### Return Values:
**mask** - The current mouse masking mode.
### RiftSlider:GetMouseoverUnit
Gets the unit that is being represented by this frame.
 
    unit = Frame:GetMouseoverUnit()   -- string <- void
#### Return Values:
**unit** - The current mouseover unit. May be nil if there is no mouseover unit.
### RiftSlider:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
### RiftSlider:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### RiftSlider:GetParent
Gets the parent of this frame.
 
    parent = Frame:GetParent()   -- Element <- void
#### Return Values:
**parent** - The parent element of this frame.
### RiftSlider:GetPosition
Returns the current position of the slider.
 
    position = RiftSlider:GetPosition()   -- number <- void
#### Return Values:
**position** - The current position of this slider.
### RiftSlider:GetRange
Returns the current range of the slider.
 
    minimum, maximum = RiftSlider:GetRange()   -- number, number <- void
#### Return Values:
**maximum** - The maximum value for the position of this slider.
**minimum** - The minimum value for the position of this slider.
### RiftSlider:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### RiftSlider:GetSecureMode
Get the current secure mode. See SetSecureMode for details.
 
    secure = Frame:GetSecureMode()   -- string <- void
#### Return Values:
**secure** - The current secure mode.
### RiftSlider:GetStrata
Gets the frame's strata. The strata determines render order on a coarser level than Layer does, as well as determining how far an element is brought to the front when clicked on.
 
    strata = Frame:GetStrata()   -- string <- void
#### Return Values:
**strata** - The item's current strata.
### RiftSlider:GetStrataList
Gets a list of valid stratas for this frame.
 
    stratas = Frame:GetStrataList()   -- table <- void
#### Return Values:
**stratas** - An array of valid stratas, in order.
### RiftSlider:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### RiftSlider:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### RiftSlider:GetVisible
Gets the visibility flag for this frame.
 
    visible = Element:GetVisible()   -- boolean <- void
#### Return Values:
**visible** - This frame's visibility flag, as set by SetVisible. Does not check the visibility flags of the frame's parents.
### RiftSlider:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### RiftSlider:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### RiftSlider:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### RiftSlider:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### RiftSlider:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### RiftSlider:SetAllPoints
Pins all the edges of this frame to the edges of a different frame. If no target is given, defaults to this frame's parent.
 
    Frame:SetAllPoints()   -- void
    Frame:SetAllPoints(target)   -- Layout
#### Parameters:
**target** - Target Layout to pin this frame's edges to.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftSlider:SetAlpha
Sets the alpha transparency multiplier for this frame and its children.
 
    Element:SetAlpha(alpha)   -- number
#### Parameters:
**alpha** - The new alpha multiplier. 1 is fully opaque, 0 is fully transparent.
### RiftSlider:SetBackgroundColor
Sets the background color of this frame.
 
    Element:SetBackgroundColor(r, g, b)   -- number, number, number
    Element:SetBackgroundColor(r, g, b, a)   -- number, number, number, number
#### Parameters:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.
**g** - Green.
### RiftSlider:SetEnabled
Sets whether the slider is enabled or grayed out.
 
    RiftSlider:SetEnabled(enabled)   -- boolean
#### Parameters:
**enabled** - The new enable state of this item.
### RiftSlider:SetHeight
Sets the height of this frame. Undefined results if the frame already has two pinned Y coordinates.
 
    Frame:SetHeight(height)   -- number
#### Parameters:
**height** - The new height of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftSlider:SetKeyFocus
Sets the key focus status. Note that only one frame can be the key focus at a time. Focusing on another frame will automatically unset the current focus.
 
    Element:SetKeyFocus(focus)   -- boolean
#### Parameters:
**focus** - The new key focus setting.
### RiftSlider:SetLayer
Sets the frame layer for this frame. This can be any number. Frames are drawn in ascending order. If two frames have the same layer number, then the order of those frames is undefined, but guarantees no Z-fighting. Frames are always drawn after their parents.
 
    Frame:SetLayer(layer)   -- number
#### Parameters:
**layer** - The new layer for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftSlider:SetMouseMasking
Sets the frame's mouse masking mode.
 
    Element:SetMouseMasking(mask)   -- string
#### Parameters:
**mask** - The new mouse masking mode. "full" is the standard mode, and means that creating any Left, Right, or movement-related mouse event will cause the frame to accept and consume any event from any of those types. "limited" causes the frame to accept and consume only events for buttons that have been hooked, so that hooking "LeftDown" will still pass Right mouse events through the frame. Note that hooking any mouse event will still consume MouseMove/In/Out events.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftSlider:SetMouseoverUnit
Sets the unit that will be represented by this frame.
 
    Frame:SetMouseoverUnit(unit)   -- unit
    Frame:SetMouseoverUnit(unit)   -- nil
#### Parameters:
**unit** - The new mouseover unit. May be a unit ID or a unit specifier. Pass in nil to disable the mouseover effect.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### RiftSlider:SetParent
Sets the parent of this frame. Circular dependencies result in undefined behavior. If this frame's secure mode is "restricted", then its parent must also have a secure mode of "restricted".
 
    Frame:SetParent(parent)   -- Element
#### Parameters:
**parent** - The new parent for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftSlider:SetPoint
Pins a point on this frame to a location on another frame. This is a rather complex function and you should look at examples to see how to use it.
This function can take many different forms. In general, it looks like this: SetPoint(point_on_this_frame, target_frame, point_on_target_frame [, x_offset, y_offset]).
The first part is the point on this frame that will be attached. Usually, these are string identifiers. "TOPLEFT", "TOPCENTER", "TOPRIGHT", "CENTERLEFT", "CENTER", "CENTERRIGHT", "BOTTOMLEFT", "BOTTOMCENTER", "BOTTOMRIGHT". You may also use a string identifier that refers to a single axis - "TOP", "BOTTOM", "LEFT", "RIGHT", "CENTERX", "CENTERY". If you want more direct numeric control you can use number pairs. 0,0 is equivalent to "TOPLEFT", 1,1 is equivalent to "BOTTOMRIGHT", 0.5,nil is equivalent to "CENTERX".
The second part is the frame to attach to, as well as the point on that frame to attach to. The frame is simply passing in the frame table. The point is the same identifier or number pair as the first parameter.
Optionally, you may include an X or Y offset to the point.
This connection is permanent, and if the target frame moves, this frame will move along with it. 
Caveat: If the target is a frame set to the "restricted" SecureMode, and the client is currently in "secure" mode, then unexpected behavior may occur.
 
    Frame:SetPoint(...)   -- ...
#### Parameters:
**...** - This function's parameters are complicated. Read the above summary for details.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftSlider:SetPosition
Changes the current position of the slider.
 
    RiftSlider:SetPosition(position)   -- number
#### Parameters:
**position** - The new position of this slider. Must be within the current range.
### RiftSlider:SetRange
Sets the current range of the slider.
 
    RiftSlider:SetRange(minimum, maximum)   -- number, number
#### Parameters:
**maximum** - The new maximum value for the position of this slider. Must be an integer and larger than or equal to "minimum".
**minimum** - The new minimum value for the position of this slider. Must be an integer and smaller than or equal to "maximum".
### RiftSlider:SetSecureMode
Sets the frame's secure mode.
"normal" is the standard mode. It allows for most functionality to be used and does not restrict frames in combat. "restricted" allows for certain sensitive functions to be called, but disables a significant amount of functionality in combat. In order to change a frame to "restricted", its parent must already be "restricted". Note that a "restricted" frame can still have "normal" children.
If you are not planning to use any restricted functions, your frame should remain in normal mode.
At the moment, it is not possible to change from "restricted" back to "normal".
 
    Frame:SetSecureMode(secure)   -- string
#### Parameters:
**secure** - The new secure mode. Valid inputs are "normal" and "restricted".
### RiftSlider:SetStrata
Sets the strata for this frame.
 
    Frame:SetStrata(strata)   -- string
#### Parameters:
**strata** - The item's new strata. Must be one of the elements returned by :GetStrataList().
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftSlider:SetVisible
Sets the frame's visibility flag. If set to false, then this frame and all its children will not be rendered or accept mouse input.
 
    Element:SetVisible(visible)   -- boolean
#### Parameters:
**visible** - The new visibility flag.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftSlider:SetWidth
Sets the width of this frame. Undefined results if the frame already has two pinned X coordinates.
 
    Frame:SetWidth(width)   -- number
#### Parameters:
**width** - The new width of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftTextfield:ClearAll
Clear all set points and sizes from this frame.
 
    Frame:ClearAll()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftTextfield:ClearHeight
Clear a set height from this frame.
 
    Frame:ClearHeight()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftTextfield:ClearPoint
Clear a set point from this frame.
 
    Frame:ClearPoint(coordinate)   -- string
    Frame:ClearPoint(x, y)   -- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point.
**x** - X coordinate of the point.
**coordinate** - Named coordinate.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftTextfield:ClearWidth
Clear a set width from this frame.
 
    Frame:ClearWidth()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftTextfield:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftTextfield:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftTextfield:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftTextfield:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### RiftTextfield:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### RiftTextfield:GetAlpha
Gets the alpha multiplier of this frame.
 
    alpha = Element:GetAlpha()   -- number <- void
#### Return Values:
**alpha** - The alpha multiplier of this frame. 1 is fully opaque, 0 is fully transparent. This does not include multiplied alphas from this frame's parent - it's the exact value passed to SetAlpha.
### RiftTextfield:GetBackgroundColor
Retrieves the background color of this frame.
 
    r, g, b, a = Element:GetBackgroundColor()   -- number, number, number, number <- void
#### Return Values:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.
**g** - Green.
### RiftTextfield:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### RiftTextfield:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### RiftTextfield:GetChildren
Returns a table containing all of this element's children.
 
    children = Element:GetChildren()   -- table <- void
#### Return Values:
**children** - A table containing this element's children.
### RiftTextfield:GetCursor
Returns the current position of the cursor.
 
    cursor = RiftTextfield:GetCursor()   -- number <- void
#### Return Values:
**cursor** - The current position of this cursor. 0 indicates a cursor placed before any text.
### RiftTextfield:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### RiftTextfield:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### RiftTextfield:GetKeyFocus
Gets the key focus status.
 
    focus = Element:GetKeyFocus()   -- boolean <- void
#### Return Values:
**focus** - Whether this frame is the current key focus.
### RiftTextfield:GetLayer
Gets the frame's layer order.
 
    layer = Frame:GetLayer()   -- number <- void
#### Return Values:
**layer** - The render layer of this frame. See Frame:SetLayer for details.
### RiftTextfield:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### RiftTextfield:GetMouseMasking
Get the current mouse masking mode. See SetMouseMasking for details.
 
    mask = Element:GetMouseMasking()   -- string <- void
#### Return Values:
**mask** - The current mouse masking mode.
### RiftTextfield:GetMouseoverUnit
Gets the unit that is being represented by this frame.
 
    unit = Frame:GetMouseoverUnit()   -- string <- void
#### Return Values:
**unit** - The current mouseover unit. May be nil if there is no mouseover unit.
### RiftTextfield:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
### RiftTextfield:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### RiftTextfield:GetParent
Gets the parent of this frame.
 
    parent = Frame:GetParent()   -- Element <- void
#### Return Values:
**parent** - The parent element of this frame.
### RiftTextfield:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### RiftTextfield:GetSecureMode
Get the current secure mode. See SetSecureMode for details.
 
    secure = Frame:GetSecureMode()   -- string <- void
#### Return Values:
**secure** - The current secure mode.
### RiftTextfield:GetSelection
Returns the current bounds of the selected text.
 
    begin, end = RiftTextfield:GetSelection()   -- number, number <- void
#### Return Values:
**begin** - The beginning of the selected text, in the same format GetCursor uses. nil if there is no text selected.
**end** - The end of the selected text, in the same format GetCursor uses. nil if there is no text selected.
### RiftTextfield:GetSelectionText
Get the current selected text for this element. Returns nil if no text has been selected.
 
    text = RiftTextfield:GetSelectionText()   -- string <- void
#### Return Values:
**text** - The current text of the element.
### RiftTextfield:GetStrata
Gets the frame's strata. The strata determines render order on a coarser level than Layer does, as well as determining how far an element is brought to the front when clicked on.
 
    strata = Frame:GetStrata()   -- string <- void
#### Return Values:
**strata** - The item's current strata.
### RiftTextfield:GetStrataList
Gets a list of valid stratas for this frame.
 
    stratas = Frame:GetStrataList()   -- table <- void
#### Return Values:
**stratas** - An array of valid stratas, in order.
### RiftTextfield:GetText
Get the current text for this element.
 
    text = RiftTextfield:GetText()   -- string <- void
#### Return Values:
**text** - The current text of the element.
### RiftTextfield:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### RiftTextfield:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### RiftTextfield:GetVisible
Gets the visibility flag for this frame.
 
    visible = Element:GetVisible()   -- boolean <- void
#### Return Values:
**visible** - This frame's visibility flag, as set by SetVisible. Does not check the visibility flags of the frame's parents.
### RiftTextfield:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### RiftTextfield:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### RiftTextfield:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### RiftTextfield:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### RiftTextfield:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### RiftTextfield:SetAllPoints
Pins all the edges of this frame to the edges of a different frame. If no target is given, defaults to this frame's parent.
 
    Frame:SetAllPoints()   -- void
    Frame:SetAllPoints(target)   -- Layout
#### Parameters:
**target** - Target Layout to pin this frame's edges to.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftTextfield:SetAlpha
Sets the alpha transparency multiplier for this frame and its children.
 
    Element:SetAlpha(alpha)   -- number
#### Parameters:
**alpha** - The new alpha multiplier. 1 is fully opaque, 0 is fully transparent.
### RiftTextfield:SetBackgroundColor
Sets the background color of this frame.
 
    Element:SetBackgroundColor(r, g, b)   -- number, number, number
    Element:SetBackgroundColor(r, g, b, a)   -- number, number, number, number
#### Parameters:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.
**g** - Green.
### RiftTextfield:SetCursor
Changes the current position of the cursor.
 
    RiftTextfield:SetCursor(cursor)   -- number
#### Parameters:
**cursor** - The new position of this cursor. Must be within the valid range.
### RiftTextfield:SetHeight
Sets the height of this frame. Undefined results if the frame already has two pinned Y coordinates.
 
    Frame:SetHeight(height)   -- number
#### Parameters:
**height** - The new height of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftTextfield:SetKeyFocus
Sets the key focus status. Note that only one frame can be the key focus at a time. Focusing on another frame will automatically unset the current focus.
 
    Element:SetKeyFocus(focus)   -- boolean
#### Parameters:
**focus** - The new key focus setting.
### RiftTextfield:SetLayer
Sets the frame layer for this frame. This can be any number. Frames are drawn in ascending order. If two frames have the same layer number, then the order of those frames is undefined, but guarantees no Z-fighting. Frames are always drawn after their parents.
 
    Frame:SetLayer(layer)   -- number
#### Parameters:
**layer** - The new layer for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftTextfield:SetMouseMasking
Sets the frame's mouse masking mode.
 
    Element:SetMouseMasking(mask)   -- string
#### Parameters:
**mask** - The new mouse masking mode. "full" is the standard mode, and means that creating any Left, Right, or movement-related mouse event will cause the frame to accept and consume any event from any of those types. "limited" causes the frame to accept and consume only events for buttons that have been hooked, so that hooking "LeftDown" will still pass Right mouse events through the frame. Note that hooking any mouse event will still consume MouseMove/In/Out events.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftTextfield:SetMouseoverUnit
Sets the unit that will be represented by this frame.
 
    Frame:SetMouseoverUnit(unit)   -- unit
    Frame:SetMouseoverUnit(unit)   -- nil
#### Parameters:
**unit** - The new mouseover unit. May be a unit ID or a unit specifier. Pass in nil to disable the mouseover effect.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### RiftTextfield:SetParent
Sets the parent of this frame. Circular dependencies result in undefined behavior. If this frame's secure mode is "restricted", then its parent must also have a secure mode of "restricted".
 
    Frame:SetParent(parent)   -- Element
#### Parameters:
**parent** - The new parent for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftTextfield:SetPoint
Pins a point on this frame to a location on another frame. This is a rather complex function and you should look at examples to see how to use it.
This function can take many different forms. In general, it looks like this: SetPoint(point_on_this_frame, target_frame, point_on_target_frame [, x_offset, y_offset]).
The first part is the point on this frame that will be attached. Usually, these are string identifiers. "TOPLEFT", "TOPCENTER", "TOPRIGHT", "CENTERLEFT", "CENTER", "CENTERRIGHT", "BOTTOMLEFT", "BOTTOMCENTER", "BOTTOMRIGHT". You may also use a string identifier that refers to a single axis - "TOP", "BOTTOM", "LEFT", "RIGHT", "CENTERX", "CENTERY". If you want more direct numeric control you can use number pairs. 0,0 is equivalent to "TOPLEFT", 1,1 is equivalent to "BOTTOMRIGHT", 0.5,nil is equivalent to "CENTERX".
The second part is the frame to attach to, as well as the point on that frame to attach to. The frame is simply passing in the frame table. The point is the same identifier or number pair as the first parameter.
Optionally, you may include an X or Y offset to the point.
This connection is permanent, and if the target frame moves, this frame will move along with it. 
Caveat: If the target is a frame set to the "restricted" SecureMode, and the client is currently in "secure" mode, then unexpected behavior may occur.
 
    Frame:SetPoint(...)   -- ...
#### Parameters:
**...** - This function's parameters are complicated. Read the above summary for details.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftTextfield:SetSecureMode
Sets the frame's secure mode.
"normal" is the standard mode. It allows for most functionality to be used and does not restrict frames in combat. "restricted" allows for certain sensitive functions to be called, but disables a significant amount of functionality in combat. In order to change a frame to "restricted", its parent must already be "restricted". Note that a "restricted" frame can still have "normal" children.
If you are not planning to use any restricted functions, your frame should remain in normal mode.
At the moment, it is not possible to change from "restricted" back to "normal".
 
    Frame:SetSecureMode(secure)   -- string
#### Parameters:
**secure** - The new secure mode. Valid inputs are "normal" and "restricted".
### RiftTextfield:SetSelection
Sets the current bounds of the selected text. Call with no arguments to remove the current selection.
 
    RiftTextfield:SetSelection()   -- void
    RiftTextfield:SetSelection(begin, end)   -- number, number
#### Parameters:
**begin** - The new beginning of the selected text, in the same format SetCursor uses. Must be an integer and smaller than "end".
**end** - The new end of the selected text, in the same format SetCursor uses. Must be an integer and larger than "begin".
### RiftTextfield:SetStrata
Sets the strata for this frame.
 
    Frame:SetStrata(strata)   -- string
#### Parameters:
**strata** - The item's new strata. Must be one of the elements returned by :GetStrataList().
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftTextfield:SetText
Set the current text for this element.
 
    RiftTextfield:SetText(text)   -- string
#### Parameters:
**text** - The new text for the element.
### RiftTextfield:SetVisible
Sets the frame's visibility flag. If set to false, then this frame and all its children will not be rendered or accept mouse input.
 
    Element:SetVisible(visible)   -- boolean
#### Parameters:
**visible** - The new visibility flag.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftTextfield:SetWidth
Sets the width of this frame. Undefined results if the frame already has two pinned X coordinates.
 
    Frame:SetWidth(width)   -- number
#### Parameters:
**width** - The new width of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:ClearAll
Clear all set points and sizes from this frame.
 
    Frame:ClearAll()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:ClearHeight
Clear a set height from this frame.
 
    Frame:ClearHeight()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:ClearPoint
Clear a set point from this frame.
 
    Frame:ClearPoint(coordinate)   -- string
    Frame:ClearPoint(x, y)   -- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point.
**x** - X coordinate of the point.
**coordinate** - Named coordinate.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:ClearWidth
Clear a set width from this frame.
 
    Frame:ClearWidth()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftWindow:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftWindow:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### RiftWindow:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### RiftWindow:GetAlpha
Gets the alpha multiplier of this frame.
 
    alpha = Element:GetAlpha()   -- number <- void
#### Return Values:
**alpha** - The alpha multiplier of this frame. 1 is fully opaque, 0 is fully transparent. This does not include multiplied alphas from this frame's parent - it's the exact value passed to SetAlpha.
### RiftWindow:GetBackgroundColor
Retrieves the background color of this frame.
 
    r, g, b, a = Element:GetBackgroundColor()   -- number, number, number, number <- void
#### Return Values:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.
**g** - Green.
### RiftWindow:GetBorder
Gets the element representing the window border.
 
    border = RiftWindow:GetBorder()   -- Element <- void
#### Return Values:
**border** - The window border.
### RiftWindow:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### RiftWindow:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### RiftWindow:GetChildren
Returns a table containing all of this element's children.
 
    children = Element:GetChildren()   -- table <- void
#### Return Values:
**children** - A table containing this element's children.
### RiftWindow:GetContent
Gets the element representing the window content area.
 
    content = RiftWindow:GetContent()   -- Element <- void
#### Return Values:
**content** - The window content area.
### RiftWindow:GetController
Gets the ID of the current controller for this window.
 
    controller = RiftWindow:GetController()   -- string <- void
#### Return Values:
**controller** - "border" or "content", whichever dictates the dimensions of the other.
### RiftWindow:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### RiftWindow:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### RiftWindow:GetKeyFocus
Gets the key focus status.
 
    focus = Element:GetKeyFocus()   -- boolean <- void
#### Return Values:
**focus** - Whether this frame is the current key focus.
### RiftWindow:GetLayer
Gets the frame's layer order.
 
    layer = Frame:GetLayer()   -- number <- void
#### Return Values:
**layer** - The render layer of this frame. See Frame:SetLayer for details.
### RiftWindow:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### RiftWindow:GetMouseMasking
Get the current mouse masking mode. See SetMouseMasking for details.
 
    mask = Element:GetMouseMasking()   -- string <- void
#### Return Values:
**mask** - The current mouse masking mode.
### RiftWindow:GetMouseoverUnit
Gets the unit that is being represented by this frame.
 
    unit = Frame:GetMouseoverUnit()   -- string <- void
#### Return Values:
**unit** - The current mouseover unit. May be nil if there is no mouseover unit.
### RiftWindow:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
### RiftWindow:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### RiftWindow:GetParent
Gets the parent of this frame.
 
    parent = Frame:GetParent()   -- Element <- void
#### Return Values:
**parent** - The parent element of this frame.
### RiftWindow:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### RiftWindow:GetSecureMode
Get the current secure mode. See SetSecureMode for details.
 
    secure = Frame:GetSecureMode()   -- string <- void
#### Return Values:
**secure** - The current secure mode.
### RiftWindow:GetStrata
Gets the frame's strata. The strata determines render order on a coarser level than Layer does, as well as determining how far an element is brought to the front when clicked on.
 
    strata = Frame:GetStrata()   -- string <- void
#### Return Values:
**strata** - The item's current strata.
### RiftWindow:GetStrataList
Gets a list of valid stratas for this frame.
 
    stratas = Frame:GetStrataList()   -- table <- void
#### Return Values:
**stratas** - An array of valid stratas, in order.
### RiftWindow:GetTitle
Get the current title for this element.
 
    title = RiftWindow:GetTitle()   -- string <- void
#### Return Values:
**title** - The current title of the element.
### RiftWindow:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### RiftWindow:GetTrimDimensions
Gets the thicknesses of the border's visual trim.
 
    left, top, right, bottom = RiftWindow:GetTrimDimensions()   -- number, number, number, number <- void
#### Return Values:
**right** - The thickness of the right window border.
**left** - The thickness of the left window border.
**bottom** - The thickness of the bottom window border.
**top** - The thickness of the top window border.
### RiftWindow:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### RiftWindow:GetVisible
Gets the visibility flag for this frame.
 
    visible = Element:GetVisible()   -- boolean <- void
#### Return Values:
**visible** - This frame's visibility flag, as set by SetVisible. Does not check the visibility flags of the frame's parents.
### RiftWindow:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### RiftWindow:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### RiftWindow:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### RiftWindow:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### RiftWindow:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### RiftWindow:SetAllPoints
Pins all the edges of this frame to the edges of a different frame. If no target is given, defaults to this frame's parent.
 
    Frame:SetAllPoints()   -- void
    Frame:SetAllPoints(target)   -- Layout
#### Parameters:
**target** - Target Layout to pin this frame's edges to.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:SetAlpha
Sets the alpha transparency multiplier for this frame and its children.
 
    Element:SetAlpha(alpha)   -- number
#### Parameters:
**alpha** - The new alpha multiplier. 1 is fully opaque, 0 is fully transparent.
### RiftWindow:SetBackgroundColor
Sets the background color of this frame.
 
    Element:SetBackgroundColor(r, g, b)   -- number, number, number
    Element:SetBackgroundColor(r, g, b, a)   -- number, number, number, number
#### Parameters:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.
**g** - Green.
### RiftWindow:SetController
Sets the current controller for this window. The controller will take on the exact dimensions of the RiftWindow object, and the other element will adjust accordingly.
 
    RiftWindow:SetController(controller)   -- string
#### Parameters:
**controller** - The new controller ID. May be either "border" or "content".
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:SetHeight
Sets the height of this frame. Undefined results if the frame already has two pinned Y coordinates.
 
    Frame:SetHeight(height)   -- number
#### Parameters:
**height** - The new height of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:SetKeyFocus
Sets the key focus status. Note that only one frame can be the key focus at a time. Focusing on another frame will automatically unset the current focus.
 
    Element:SetKeyFocus(focus)   -- boolean
#### Parameters:
**focus** - The new key focus setting.
### RiftWindow:SetLayer
Sets the frame layer for this frame. This can be any number. Frames are drawn in ascending order. If two frames have the same layer number, then the order of those frames is undefined, but guarantees no Z-fighting. Frames are always drawn after their parents.
 
    Frame:SetLayer(layer)   -- number
#### Parameters:
**layer** - The new layer for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:SetMouseMasking
Sets the frame's mouse masking mode.
 
    Element:SetMouseMasking(mask)   -- string
#### Parameters:
**mask** - The new mouse masking mode. "full" is the standard mode, and means that creating any Left, Right, or movement-related mouse event will cause the frame to accept and consume any event from any of those types. "limited" causes the frame to accept and consume only events for buttons that have been hooked, so that hooking "LeftDown" will still pass Right mouse events through the frame. Note that hooking any mouse event will still consume MouseMove/In/Out events.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:SetMouseoverUnit
Sets the unit that will be represented by this frame.
 
    Frame:SetMouseoverUnit(unit)   -- unit
    Frame:SetMouseoverUnit(unit)   -- nil
#### Parameters:
**unit** - The new mouseover unit. May be a unit ID or a unit specifier. Pass in nil to disable the mouseover effect.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### RiftWindow:SetParent
Sets the parent of this frame. Circular dependencies result in undefined behavior. If this frame's secure mode is "restricted", then its parent must also have a secure mode of "restricted".
 
    Frame:SetParent(parent)   -- Element
#### Parameters:
**parent** - The new parent for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:SetPoint
Pins a point on this frame to a location on another frame. This is a rather complex function and you should look at examples to see how to use it.
This function can take many different forms. In general, it looks like this: SetPoint(point_on_this_frame, target_frame, point_on_target_frame [, x_offset, y_offset]).
The first part is the point on this frame that will be attached. Usually, these are string identifiers. "TOPLEFT", "TOPCENTER", "TOPRIGHT", "CENTERLEFT", "CENTER", "CENTERRIGHT", "BOTTOMLEFT", "BOTTOMCENTER", "BOTTOMRIGHT". You may also use a string identifier that refers to a single axis - "TOP", "BOTTOM", "LEFT", "RIGHT", "CENTERX", "CENTERY". If you want more direct numeric control you can use number pairs. 0,0 is equivalent to "TOPLEFT", 1,1 is equivalent to "BOTTOMRIGHT", 0.5,nil is equivalent to "CENTERX".
The second part is the frame to attach to, as well as the point on that frame to attach to. The frame is simply passing in the frame table. The point is the same identifier or number pair as the first parameter.
Optionally, you may include an X or Y offset to the point.
This connection is permanent, and if the target frame moves, this frame will move along with it. 
Caveat: If the target is a frame set to the "restricted" SecureMode, and the client is currently in "secure" mode, then unexpected behavior may occur.
 
    Frame:SetPoint(...)   -- ...
#### Parameters:
**...** - This function's parameters are complicated. Read the above summary for details.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:SetSecureMode
Sets the frame's secure mode.
"normal" is the standard mode. It allows for most functionality to be used and does not restrict frames in combat. "restricted" allows for certain sensitive functions to be called, but disables a significant amount of functionality in combat. In order to change a frame to "restricted", its parent must already be "restricted". Note that a "restricted" frame can still have "normal" children.
If you are not planning to use any restricted functions, your frame should remain in normal mode.
At the moment, it is not possible to change from "restricted" back to "normal".
 
    Frame:SetSecureMode(secure)   -- string
#### Parameters:
**secure** - The new secure mode. Valid inputs are "normal" and "restricted".
### RiftWindow:SetStrata
Sets the strata for this frame.
 
    Frame:SetStrata(strata)   -- string
#### Parameters:
**strata** - The item's new strata. Must be one of the elements returned by :GetStrataList().
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:SetTitle
Set the current title for this element.
 
    RiftWindow:SetTitle(title)   -- string
#### Parameters:
**title** - The new title for the element.
### RiftWindow:SetVisible
Sets the frame's visibility flag. If set to false, then this frame and all its children will not be rendered or accept mouse input.
 
    Element:SetVisible(visible)   -- boolean
#### Parameters:
**visible** - The new visibility flag.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindow:SetWidth
Sets the width of this frame. Undefined results if the frame already has two pinned X coordinates.
 
    Frame:SetWidth(width)   -- number
#### Parameters:
**width** - The new width of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindowBorder:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindowBorder:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftWindowBorder:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftWindowBorder:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### RiftWindowBorder:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### RiftWindowBorder:GetAlpha
Gets the alpha multiplier of this frame.
 
    alpha = Element:GetAlpha()   -- number <- void
#### Return Values:
**alpha** - The alpha multiplier of this frame. 1 is fully opaque, 0 is fully transparent. This does not include multiplied alphas from this frame's parent - it's the exact value passed to SetAlpha.
### RiftWindowBorder:GetBackgroundColor
Retrieves the background color of this frame.
 
    r, g, b, a = Element:GetBackgroundColor()   -- number, number, number, number <- void
#### Return Values:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.
**g** - Green.
### RiftWindowBorder:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### RiftWindowBorder:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### RiftWindowBorder:GetChildren
Returns a table containing all of this element's children.
 
    children = Element:GetChildren()   -- table <- void
#### Return Values:
**children** - A table containing this element's children.
### RiftWindowBorder:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### RiftWindowBorder:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### RiftWindowBorder:GetKeyFocus
Gets the key focus status.
 
    focus = Element:GetKeyFocus()   -- boolean <- void
#### Return Values:
**focus** - Whether this frame is the current key focus.
### RiftWindowBorder:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### RiftWindowBorder:GetMouseMasking
Get the current mouse masking mode. See SetMouseMasking for details.
 
    mask = Element:GetMouseMasking()   -- string <- void
#### Return Values:
**mask** - The current mouse masking mode.
### RiftWindowBorder:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
### RiftWindowBorder:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### RiftWindowBorder:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### RiftWindowBorder:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### RiftWindowBorder:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### RiftWindowBorder:GetVisible
Gets the visibility flag for this frame.
 
    visible = Element:GetVisible()   -- boolean <- void
#### Return Values:
**visible** - This frame's visibility flag, as set by SetVisible. Does not check the visibility flags of the frame's parents.
### RiftWindowBorder:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### RiftWindowBorder:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### RiftWindowBorder:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### RiftWindowBorder:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### RiftWindowBorder:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### RiftWindowBorder:SetAlpha
Sets the alpha transparency multiplier for this frame and its children.
 
    Element:SetAlpha(alpha)   -- number
#### Parameters:
**alpha** - The new alpha multiplier. 1 is fully opaque, 0 is fully transparent.
### RiftWindowBorder:SetBackgroundColor
Sets the background color of this frame.
 
    Element:SetBackgroundColor(r, g, b)   -- number, number, number
    Element:SetBackgroundColor(r, g, b, a)   -- number, number, number, number
#### Parameters:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.
**g** - Green.
### RiftWindowBorder:SetKeyFocus
Sets the key focus status. Note that only one frame can be the key focus at a time. Focusing on another frame will automatically unset the current focus.
 
    Element:SetKeyFocus(focus)   -- boolean
#### Parameters:
**focus** - The new key focus setting.
### RiftWindowBorder:SetMouseMasking
Sets the frame's mouse masking mode.
 
    Element:SetMouseMasking(mask)   -- string
#### Parameters:
**mask** - The new mouse masking mode. "full" is the standard mode, and means that creating any Left, Right, or movement-related mouse event will cause the frame to accept and consume any event from any of those types. "limited" causes the frame to accept and consume only events for buttons that have been hooked, so that hooking "LeftDown" will still pass Right mouse events through the frame. Note that hooking any mouse event will still consume MouseMove/In/Out events.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindowBorder:SetVisible
Sets the frame's visibility flag. If set to false, then this frame and all its children will not be rendered or accept mouse input.
 
    Element:SetVisible(visible)   -- boolean
#### Parameters:
**visible** - The new visibility flag.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindowContent:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindowContent:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftWindowContent:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### RiftWindowContent:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### RiftWindowContent:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### RiftWindowContent:GetAlpha
Gets the alpha multiplier of this frame.
 
    alpha = Element:GetAlpha()   -- number <- void
#### Return Values:
**alpha** - The alpha multiplier of this frame. 1 is fully opaque, 0 is fully transparent. This does not include multiplied alphas from this frame's parent - it's the exact value passed to SetAlpha.
### RiftWindowContent:GetBackgroundColor
Retrieves the background color of this frame.
 
    r, g, b, a = Element:GetBackgroundColor()   -- number, number, number, number <- void
#### Return Values:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.
**g** - Green.
### RiftWindowContent:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### RiftWindowContent:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### RiftWindowContent:GetChildren
Returns a table containing all of this element's children.
 
    children = Element:GetChildren()   -- table <- void
#### Return Values:
**children** - A table containing this element's children.
### RiftWindowContent:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### RiftWindowContent:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### RiftWindowContent:GetKeyFocus
Gets the key focus status.
 
    focus = Element:GetKeyFocus()   -- boolean <- void
#### Return Values:
**focus** - Whether this frame is the current key focus.
### RiftWindowContent:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### RiftWindowContent:GetMouseMasking
Get the current mouse masking mode. See SetMouseMasking for details.
 
    mask = Element:GetMouseMasking()   -- string <- void
#### Return Values:
**mask** - The current mouse masking mode.
### RiftWindowContent:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
### RiftWindowContent:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### RiftWindowContent:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### RiftWindowContent:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### RiftWindowContent:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### RiftWindowContent:GetVisible
Gets the visibility flag for this frame.
 
    visible = Element:GetVisible()   -- boolean <- void
#### Return Values:
**visible** - This frame's visibility flag, as set by SetVisible. Does not check the visibility flags of the frame's parents.
### RiftWindowContent:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### RiftWindowContent:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### RiftWindowContent:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### RiftWindowContent:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### RiftWindowContent:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### RiftWindowContent:SetAlpha
Sets the alpha transparency multiplier for this frame and its children.
 
    Element:SetAlpha(alpha)   -- number
#### Parameters:
**alpha** - The new alpha multiplier. 1 is fully opaque, 0 is fully transparent.
### RiftWindowContent:SetBackgroundColor
Sets the background color of this frame.
 
    Element:SetBackgroundColor(r, g, b)   -- number, number, number
    Element:SetBackgroundColor(r, g, b, a)   -- number, number, number, number
#### Parameters:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.
**g** - Green.
### RiftWindowContent:SetKeyFocus
Sets the key focus status. Note that only one frame can be the key focus at a time. Focusing on another frame will automatically unset the current focus.
 
    Element:SetKeyFocus(focus)   -- boolean
#### Parameters:
**focus** - The new key focus setting.
### RiftWindowContent:SetMouseMasking
Sets the frame's mouse masking mode.
 
    Element:SetMouseMasking(mask)   -- string
#### Parameters:
**mask** - The new mouse masking mode. "full" is the standard mode, and means that creating any Left, Right, or movement-related mouse event will cause the frame to accept and consume any event from any of those types. "limited" causes the frame to accept and consume only events for buttons that have been hooked, so that hooking "LeftDown" will still pass Right mouse events through the frame. Note that hooking any mouse event will still consume MouseMove/In/Out events.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### RiftWindowContent:SetVisible
Sets the frame's visibility flag. If set to false, then this frame and all its children will not be rendered or accept mouse input.
 
    Element:SetVisible(visible)   -- boolean
#### Parameters:
**visible** - The new visibility flag.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:ClearAll
Clear all set points and sizes from this frame.
 
    Frame:ClearAll()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:ClearHeight
Clear a set height from this frame.
 
    Frame:ClearHeight()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:ClearPoint
Clear a set point from this frame.
 
    Frame:ClearPoint(coordinate)   -- string
    Frame:ClearPoint(x, y)   -- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point.
**x** - X coordinate of the point.
**coordinate** - Named coordinate.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:ClearWidth
Clear a set width from this frame.
 
    Frame:ClearWidth()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
### Text:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### Text:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### Text:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### Text:GetAlpha
Gets the alpha multiplier of this frame.
 
    alpha = Element:GetAlpha()   -- number <- void
#### Return Values:
**alpha** - The alpha multiplier of this frame. 1 is fully opaque, 0 is fully transparent. This does not include multiplied alphas from this frame's parent - it's the exact value passed to SetAlpha.
### Text:GetBackgroundColor
Retrieves the background color of this frame.
 
    r, g, b, a = Element:GetBackgroundColor()   -- number, number, number, number <- void
#### Return Values:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.
**g** - Green.
### Text:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### Text:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### Text:GetChildren
Returns a table containing all of this element's children.
 
    children = Element:GetChildren()   -- table <- void
#### Return Values:
**children** - A table containing this element's children.
### Text:GetEffectBlur
Gets the current parameters for the Blur effect.
 
    effect = Text:GetEffectBlur()   -- table <- void
    effect = Text:GetEffectBlur()   -- nil <- void
#### Return Values:
**effect** - The current parameters for this effect. All members are optional. Pass nil to disable the effect.
#### Returned Members:
**blurY** - Controls how much blurring exists along the Y axis. Defaults to 2.
**blurX** - Controls how much blurring exists along the X axis. Defaults to 2.
### Text:GetEffectGlow
Gets the current parameters for the Glow effect.
 
    effect = Text:GetEffectGlow()   -- table <- void
    effect = Text:GetEffectGlow()   -- nil <- void
#### Return Values:
**effect** - The current parameters for this effect. All members are optional. Pass nil to disable the effect.
#### Returned Members:
**colorA** - Controls the alpha channel of the glow effect. Defaults to 1.
**offsetY** - Controls the glow offset along the Y axis. Defaults to 0.
**blurX** - Controls how much blurring exists along the X axis. Defaults to 2.
**strength** - Controls the strength of the glow. Defaults to 1.
**offsetX** - Controls the glow offset along the X axis. Defaults to 0.
**replace** - Hides the original text. Defaults to false.
**knockout** - Enables the "knockout" effect. Defaults to false.
**colorG** - Controls the green channel of the glow effect. Defaults to 0.
**colorR** - Controls the red channel of the glow effect. Defaults to 0.
**colorB** - Controls the blue channel of the glow effect. Defaults to 0.
**blurY** - Controls how much blurring exists along the Y axis. Defaults to 2.
### Text:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### Text:GetFont
Gets the current font used for this element.
 
    source, font = Text:GetFont()   -- string, string <- void
#### Return Values:
**font** - The actual font identifier. Either a resource identifier or a filename.
**source** - The source of the resource. "Rift" will take the resource from Rift's internal data. Anything else will take the resource from the addon with that identifier.
### Text:GetFontColor
Gets the current font color for this element.
 
    r, g, b, a = Text:GetFontColor()   -- number, number, number, number <- void
#### Return Values:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.
**g** - Green.
### Text:GetFontSize
Gets the font size of the current element.
 
    fontsize = Text:GetFontSize()   -- number <- void
#### Return Values:
**fontsize** - The current font size of this element.
### Text:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### Text:GetKeyFocus
Gets the key focus status.
 
    focus = Element:GetKeyFocus()   -- boolean <- void
#### Return Values:
**focus** - Whether this frame is the current key focus.
### Text:GetLayer
Gets the frame's layer order.
 
    layer = Frame:GetLayer()   -- number <- void
#### Return Values:
**layer** - The render layer of this frame. See Frame:SetLayer for details.
### Text:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### Text:GetMouseMasking
Get the current mouse masking mode. See SetMouseMasking for details.
 
    mask = Element:GetMouseMasking()   -- string <- void
#### Return Values:
**mask** - The current mouse masking mode.
### Text:GetMouseoverUnit
Gets the unit that is being represented by this frame.
 
    unit = Frame:GetMouseoverUnit()   -- string <- void
#### Return Values:
**unit** - The current mouseover unit. May be nil if there is no mouseover unit.
### Text:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
### Text:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### Text:GetParent
Gets the parent of this frame.
 
    parent = Frame:GetParent()   -- Element <- void
#### Return Values:
**parent** - The parent element of this frame.
### Text:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### Text:GetSecureMode
Get the current secure mode. See SetSecureMode for details.
 
    secure = Frame:GetSecureMode()   -- string <- void
#### Return Values:
**secure** - The current secure mode.
### Text:GetStrata
Gets the frame's strata. The strata determines render order on a coarser level than Layer does, as well as determining how far an element is brought to the front when clicked on.
 
    strata = Frame:GetStrata()   -- string <- void
#### Return Values:
**strata** - The item's current strata.
### Text:GetStrataList
Gets a list of valid stratas for this frame.
 
    stratas = Frame:GetStrataList()   -- table <- void
#### Return Values:
**stratas** - An array of valid stratas, in order.
### Text:GetText
Get the current text for this element.
 
    text, html = Text:GetText()   -- string, boolean <- void
#### Return Values:
**text** - The current text of the element.
**html** - The current HTML flag.
### Text:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### Text:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### Text:GetVisible
Gets the visibility flag for this frame.
 
    visible = Element:GetVisible()   -- boolean <- void
#### Return Values:
**visible** - This frame's visibility flag, as set by SetVisible. Does not check the visibility flags of the frame's parents.
### Text:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### Text:GetWordwrap
Gets the wordwrap flag for this element.
 
    wordwrap = Text:GetWordwrap()   -- boolean <- void
#### Return Values:
**wordwrap** - The current wordwrap flag for this element. If false, long lines will be truncated. Defaults to false.
### Text:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### Text:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### Text:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### Text:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### Text:SetAllPoints
Pins all the edges of this frame to the edges of a different frame. If no target is given, defaults to this frame's parent.
 
    Frame:SetAllPoints()   -- void
    Frame:SetAllPoints(target)   -- Layout
#### Parameters:
**target** - Target Layout to pin this frame's edges to.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:SetAlpha
Sets the alpha transparency multiplier for this frame and its children.
 
    Element:SetAlpha(alpha)   -- number
#### Parameters:
**alpha** - The new alpha multiplier. 1 is fully opaque, 0 is fully transparent.
### Text:SetBackgroundColor
Sets the background color of this frame.
 
    Element:SetBackgroundColor(r, g, b)   -- number, number, number
    Element:SetBackgroundColor(r, g, b, a)   -- number, number, number, number
#### Parameters:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.
**g** - Green.
### Text:SetEffectBlur
Sets the parameters for the Blur effect.
 
    Text:SetEffectBlur(effect)   -- table
    Text:SetEffectBlur(effect)   -- nil
#### Parameters:
**effect** - The parameters for this effect. All members are optional. Pass nil to disable the effect.
#### Returned Members:
**blurY** - Controls how much blurring exists along the Y axis. Defaults to 2.
**blurX** - Controls how much blurring exists along the X axis. Defaults to 2.
### Text:SetEffectGlow
Sets the parameters for the Glow effect.
 
    Text:SetEffectGlow(effect)   -- table
    Text:SetEffectGlow(effect)   -- nil
#### Parameters:
**effect** - The parameters for this effect. All members are optional. Pass nil to disable the effect.
#### Returned Members:
**colorA** - Controls the alpha channel of the glow effect. Defaults to 1.
**offsetY** - Controls the glow offset along the Y axis. Defaults to 0.
**blurX** - Controls how much blurring exists along the X axis. Defaults to 2.
**strength** - Controls the strength of the glow. Defaults to 1.
**offsetX** - Controls the glow offset along the X axis. Defaults to 0.
**replace** - Hides the original text. Defaults to false.
**knockout** - Enables the "knockout" effect. Defaults to false.
**colorG** - Controls the green channel of the glow effect. Defaults to 0.
**colorR** - Controls the red channel of the glow effect. Defaults to 0.
**colorB** - Controls the blue channel of the glow effect. Defaults to 0.
**blurY** - Controls how much blurring exists along the Y axis. Defaults to 2.
### Text:SetFont
Sets the current font used for this element.
 
    Text:SetFont(source, font)   -- string, string
#### Parameters:
**font** - The actual font identifier. Either a resource identifier or a filename.
**source** - The source of the resource. "Rift" will take the resource from Rift's internal data. Anything else will take the resource from the addon with that identifier.
### Text:SetFontColor
Sets the current font color for this element.
 
    Text:SetFontColor(r, g, b)   -- number, number, number
    Text:SetFontColor(r, g, b, a)   -- number, number, number, number
#### Parameters:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.
**g** - Green.
### Text:SetFontSize
Sets the current font size of this element.
 
    Text:SetFontSize(fontsize)   -- number
#### Parameters:
**fontsize** - The new font size of this element.
### Text:SetHeight
Sets the height of this frame. Undefined results if the frame already has two pinned Y coordinates.
 
    Frame:SetHeight(height)   -- number
#### Parameters:
**height** - The new height of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:SetKeyFocus
Sets the key focus status. Note that only one frame can be the key focus at a time. Focusing on another frame will automatically unset the current focus.
 
    Element:SetKeyFocus(focus)   -- boolean
#### Parameters:
**focus** - The new key focus setting.
### Text:SetLayer
Sets the frame layer for this frame. This can be any number. Frames are drawn in ascending order. If two frames have the same layer number, then the order of those frames is undefined, but guarantees no Z-fighting. Frames are always drawn after their parents.
 
    Frame:SetLayer(layer)   -- number
#### Parameters:
**layer** - The new layer for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:SetMouseMasking
Sets the frame's mouse masking mode.
 
    Element:SetMouseMasking(mask)   -- string
#### Parameters:
**mask** - The new mouse masking mode. "full" is the standard mode, and means that creating any Left, Right, or movement-related mouse event will cause the frame to accept and consume any event from any of those types. "limited" causes the frame to accept and consume only events for buttons that have been hooked, so that hooking "LeftDown" will still pass Right mouse events through the frame. Note that hooking any mouse event will still consume MouseMove/In/Out events.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:SetMouseoverUnit
Sets the unit that will be represented by this frame.
 
    Frame:SetMouseoverUnit(unit)   -- unit
    Frame:SetMouseoverUnit(unit)   -- nil
#### Parameters:
**unit** - The new mouseover unit. May be a unit ID or a unit specifier. Pass in nil to disable the mouseover effect.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### Text:SetParent
Sets the parent of this frame. Circular dependencies result in undefined behavior. If this frame's secure mode is "restricted", then its parent must also have a secure mode of "restricted".
 
    Frame:SetParent(parent)   -- Element
#### Parameters:
**parent** - The new parent for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:SetPoint
Pins a point on this frame to a location on another frame. This is a rather complex function and you should look at examples to see how to use it.
This function can take many different forms. In general, it looks like this: SetPoint(point_on_this_frame, target_frame, point_on_target_frame [, x_offset, y_offset]).
The first part is the point on this frame that will be attached. Usually, these are string identifiers. "TOPLEFT", "TOPCENTER", "TOPRIGHT", "CENTERLEFT", "CENTER", "CENTERRIGHT", "BOTTOMLEFT", "BOTTOMCENTER", "BOTTOMRIGHT". You may also use a string identifier that refers to a single axis - "TOP", "BOTTOM", "LEFT", "RIGHT", "CENTERX", "CENTERY". If you want more direct numeric control you can use number pairs. 0,0 is equivalent to "TOPLEFT", 1,1 is equivalent to "BOTTOMRIGHT", 0.5,nil is equivalent to "CENTERX".
The second part is the frame to attach to, as well as the point on that frame to attach to. The frame is simply passing in the frame table. The point is the same identifier or number pair as the first parameter.
Optionally, you may include an X or Y offset to the point.
This connection is permanent, and if the target frame moves, this frame will move along with it. 
Caveat: If the target is a frame set to the "restricted" SecureMode, and the client is currently in "secure" mode, then unexpected behavior may occur.
 
    Frame:SetPoint(...)   -- ...
#### Parameters:
**...** - This function's parameters are complicated. Read the above summary for details.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:SetSecureMode
Sets the frame's secure mode.
"normal" is the standard mode. It allows for most functionality to be used and does not restrict frames in combat. "restricted" allows for certain sensitive functions to be called, but disables a significant amount of functionality in combat. In order to change a frame to "restricted", its parent must already be "restricted". Note that a "restricted" frame can still have "normal" children.
If you are not planning to use any restricted functions, your frame should remain in normal mode.
At the moment, it is not possible to change from "restricted" back to "normal".
 
    Frame:SetSecureMode(secure)   -- string
#### Parameters:
**secure** - The new secure mode. Valid inputs are "normal" and "restricted".
### Text:SetStrata
Sets the strata for this frame.
 
    Frame:SetStrata(strata)   -- string
#### Parameters:
**strata** - The item's new strata. Must be one of the elements returned by :GetStrataList().
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:SetText
Sets the current text for this element.
 
    Text:SetText(text)   -- string
    Text:SetText(text, html)   -- string, boolean
#### Parameters:
**text** - The new text for the element.
**html** - Enables HTML mode. In HTML mode, a limited number of formatting tags are available: &lt;u>, &lt;font color="#rrggbb">, and &lt;a lua="print('This is a lua script.')">.
### Text:SetVisible
Sets the frame's visibility flag. If set to false, then this frame and all its children will not be rendered or accept mouse input.
 
    Element:SetVisible(visible)   -- boolean
#### Parameters:
**visible** - The new visibility flag.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:SetWidth
Sets the width of this frame. Undefined results if the frame already has two pinned X coordinates.
 
    Frame:SetWidth(width)   -- number
#### Parameters:
**width** - The new width of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Text:SetWordwrap
Sets the wordwrap flag for this element.
 
    Text:SetWordwrap(wordwrap)   -- boolean
#### Parameters:
**wordwrap** - The new wordwrap flag for this element. If false, long lines will be truncated. Defaults to false.
### Texture:ClearAll
Clear all set points and sizes from this frame.
 
    Frame:ClearAll()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Texture:ClearHeight
Clear a set height from this frame.
 
    Frame:ClearHeight()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Texture:ClearPoint
Clear a set point from this frame.
 
    Frame:ClearPoint(coordinate)   -- string
    Frame:ClearPoint(x, y)   -- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point.
**x** - X coordinate of the point.
**coordinate** - Named coordinate.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Texture:ClearWidth
Clear a set width from this frame.
 
    Frame:ClearWidth()   -- void
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Texture:EventAttach
Attaches an event handler to an event.
 
    Layout:EventAttach(handle, callback, label)   -- eventFrame, function, string
    Layout:EventAttach(handle, callback, label, priority)   -- eventFrame, function, string, number
#### Parameters:
**callback** - A global event handler function. This will be called when the event fires. The first parameter will be the frame that the event is called on, the second parameter will be the standard frame event handle, any other parameters will follow that.
**priority** - Priority of the event handler. Higher numbers trigger first.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Texture:EventDetach
Detaches an event handler from an event. Any parameter can be 'nil', and this is interpreted as a wildcard. If multiple events match the constraints, only one will be detached.
 
    Layout:EventDetach(handle, callback)   -- eventFrame, function/nil
    Layout:EventDetach(handle, callback, label)   -- eventFrame, function/nil, string/nil
    Layout:EventDetach(handle, callback, label, priority)   -- eventFrame, function/nil, string/nil, number/nil
    Layout:EventDetach(handle, callback, label, priority, owner)   -- eventFrame, function/nil, string/nil, number/nil, string/nil
#### Parameters:
**owner** - Owner to search for.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
**callback** - A callback function to search for.
**priority** - Priority of the event handler. Higher numbers trigger first.
### Texture:EventList
Lists the current event handlers for an event.
 
    result = Layout:EventList(handle)   -- table <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**result** - A table of event handlers for this event.
#### Returned Members:
**owner** - Owner to search for.
**macro** - The macro that will run when the event fires.
**label** - Human-readable label used to identify the handler in error reports, performance reports, and for later detaching.
**handler** - The handler that will be called when the event fires.
**priority** - Priority of the event handler. Higher numbers trigger first.
### Texture:EventMacroGet
Gets the macro that will be triggered when this event occurs.
 
    macro = Layout:EventMacroGet(handle)   -- string/nil <- eventFrame
#### Parameters:
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Return Values:
**macro** - The macro that will be triggered.
### Texture:EventMacroSet
Sets the macro that will be triggered when this event occurs.
 
    Layout:EventMacroSet(handle, macro)   -- eventFrame, string/nil
#### Parameters:
**macro** - The macro to trigger. nil to clear the macro.
**handle** - A handle to a frame event, usually pulled out of the "Event.UI." hierarchy.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### Texture:GetAlpha
Gets the alpha multiplier of this frame.
 
    alpha = Element:GetAlpha()   -- number <- void
#### Return Values:
**alpha** - The alpha multiplier of this frame. 1 is fully opaque, 0 is fully transparent. This does not include multiplied alphas from this frame's parent - it's the exact value passed to SetAlpha.
### Texture:GetBackgroundColor
Retrieves the background color of this frame.
 
    r, g, b, a = Element:GetBackgroundColor()   -- number, number, number, number <- void
#### Return Values:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent.
**g** - Green.
### Texture:GetBottom
Retrieves the Y position of the bottom edge of this element.
 
    bottom = Layout:GetBottom()   -- number <- void
#### Return Values:
**bottom** - The Y position of the bottom edge of this element.
### Texture:GetBounds
Retrieves the complete bounds of this element.
 
    left, top, right, bottom = Layout:GetBounds()   -- number, number, number, number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
**left** - The X position of the left edge of this element.
**bottom** - The Y position of the bottom edge of this element.
**top** - The Y position of the top edge of this element.
### Texture:GetChildren
Returns a table containing all of this element's children.
 
    children = Element:GetChildren()   -- table <- void
#### Return Values:
**children** - A table containing this element's children.
### Texture:GetEventTable
Retrieves the event table of this element. By default, this value is also stored in "this.Event".
 
    eventTable = Layout:GetEventTable()   -- table <- void
#### Return Values:
**eventTable** - The event table of this element.
### Texture:GetHeight
Retrieves the height of this element.
 
    height = Layout:GetHeight()   -- number <- void
#### Return Values:
**height** - The height of this element.
### Texture:GetKeyFocus
Gets the key focus status.
 
    focus = Element:GetKeyFocus()   -- boolean <- void
#### Return Values:
**focus** - Whether this frame is the current key focus.
### Texture:GetLayer
Gets the frame's layer order.
 
    layer = Frame:GetLayer()   -- number <- void
#### Return Values:
**layer** - The render layer of this frame. See Frame:SetLayer for details.
### Texture:GetLeft
Retrieves the X position of the left edge of this element.
 
    left = Layout:GetLeft()   -- number <- void
#### Return Values:
**left** - The X position of the left edge of this element.
### Texture:GetMouseMasking
Get the current mouse masking mode. See SetMouseMasking for details.
 
    mask = Element:GetMouseMasking()   -- string <- void
#### Return Values:
**mask** - The current mouse masking mode.
### Texture:GetMouseoverUnit
Gets the unit that is being represented by this frame.
 
    unit = Frame:GetMouseoverUnit()   -- string <- void
#### Return Values:
**unit** - The current mouseover unit. May be nil if there is no mouseover unit.
### Texture:GetName
Retrieves the name of this element.
 
    name = Layout:GetName()   -- string <- void
#### Return Values:
**name** - The name of this element, as provided by the addon that created it.
### Texture:GetOwner
Retrieves the owner of this element.
 
    owner = Layout:GetOwner()   -- string <- void
#### Return Values:
**owner** - The owner of this element. Given as an addon identifier.
### Texture:GetParent
Gets the parent of this frame.
 
    parent = Frame:GetParent()   -- Element <- void
#### Return Values:
**parent** - The parent element of this frame.
### Texture:GetRight
Retrieves the X position of the right edge of this element.
 
    right = Layout:GetRight()   -- number <- void
#### Return Values:
**right** - The X position of the right edge of this element.
### Texture:GetSecureMode
Get the current secure mode. See SetSecureMode for details.
 
    secure = Frame:GetSecureMode()   -- string <- void
#### Return Values:
**secure** - The current secure mode.
### Texture:GetStrata
Gets the frame's strata. The strata determines render order on a coarser level than Layer does, as well as determining how far an element is brought to the front when clicked on.
 
    strata = Frame:GetStrata()   -- string <- void
#### Return Values:
**strata** - The item's current strata.
### Texture:GetStrataList
Gets a list of valid stratas for this frame.
 
    stratas = Frame:GetStrataList()   -- table <- void
#### Return Values:
**stratas** - An array of valid stratas, in order.
### Texture:GetTexture
Gets the current texture used for this element.
 
    source, texture = Texture:GetTexture()   -- string, string <- void
#### Return Values:
**source** - The source of the resource. "Rift" will take the resource from Rift's internal data. Anything else will take the resource from the addon with that identifier.
**texture** - The actual texture identifier. Either a resource identifier or a filename.
### Texture:GetTextureHeight
Returns the actual pixel height of the current texture.
 
    height = Texture:GetTextureHeight()   -- number <- void
#### Return Values:
**height** - The height of the current texture in pixels.
### Texture:GetTextureWidth
Returns the actual pixel width of the current texture.
 
    width = Texture:GetTextureWidth()   -- number <- void
#### Return Values:
**width** - The width of the current texture in pixels.
### Texture:GetTop
Retrieves the Y position of the top edge of this element.
 
    top = Layout:GetTop()   -- number <- void
#### Return Values:
**top** - The Y position of the top edge of this element.
### Texture:GetType
Retrieves the type of this element.
 
    type = Layout:GetType()   -- string <- void
#### Return Values:
**type** - The type of this element.
### Texture:GetVisible
Gets the visibility flag for this frame.
 
    visible = Element:GetVisible()   -- boolean <- void
#### Return Values:
**visible** - This frame's visibility flag, as set by SetVisible. Does not check the visibility flags of the frame's parents.
### Texture:GetWidth
Retrieves the width of this element.
 
    width = Layout:GetWidth()   -- number <- void
#### Return Values:
**width** - The width of this element.
### Texture:ReadAll
Read all set points and sizes from this frame.
 
    results = Layout:ReadAll()   -- table <- void
#### Return Values:
**results** - Result table. Contains data in the following format: {x = {size = (size), [(position)] = {layout = (layout), position = (position), offset = (offset)}}, y = (the same thing)}.
### Texture:ReadHeight
Read a set height from this frame.
 
    height = Layout:ReadHeight()   -- number <- void
#### Return Values:
**height** - The parameter passed to SetHeight(), or nil if no such parameter has been passed.
### Texture:ReadPoint
Read a set point from this frame. Must be given a single-axis coordinate.
 
    layout, position, offset = Layout:ReadPoint(coordinate)   -- Layout, number, number <- string
    layout, position, offset = Layout:ReadPoint(x, y)   -- Layout, number, number <- number/nil, number/nil
    origin, offset = Layout:ReadPoint(coordinate)   -- string, number <- string
    origin, offset = Layout:ReadPoint(x, y)   -- string, number <- number/nil, number/nil
#### Parameters:
**y** - Y coordinate of the point. Either this or X must be nil.
**x** - X coordinate of the point. Either this or Y must be nil.
**coordinate** - Named coordinate. Must be a one-axis coordinate.
#### Return Values:
**position** - The position on "layout" that this point is pinned. 0 refers to the top or left edge, 1 refers to the bottom or right edge.
**offset** - The offset in pixels from the source location to the actual location.
**layout** - The table that this point is pinned to.
**origin** - The string "origin".
### Texture:ReadWidth
Read a set width from this frame.
 
    width = Layout:ReadWidth()   -- number <- void
#### Return Values:
**width** - The parameter passed to SetWidth(), or nil if no such parameter has been passed.
### Texture:SetAllPoints
Pins all the edges of this frame to the edges of a different frame. If no target is given, defaults to this frame's parent.
 
    Frame:SetAllPoints()   -- void
    Frame:SetAllPoints(target)   -- Layout
#### Parameters:
**target** - Target Layout to pin this frame's edges to.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Texture:SetAlpha
Sets the alpha transparency multiplier for this frame and its children.
 
    Element:SetAlpha(alpha)   -- number
#### Parameters:
**alpha** - The new alpha multiplier. 1 is fully opaque, 0 is fully transparent.
### Texture:SetBackgroundColor
Sets the background color of this frame.
 
    Element:SetBackgroundColor(r, g, b)   -- number, number, number
    Element:SetBackgroundColor(r, g, b, a)   -- number, number, number, number
#### Parameters:
**b** - Blue.
**r** - Red.
**a** - Alpha. 1 is fully opaque, 0 is fully transparent. Defaults to 1.
**g** - Green.
### Texture:SetHeight
Sets the height of this frame. Undefined results if the frame already has two pinned Y coordinates.
 
    Frame:SetHeight(height)   -- number
#### Parameters:
**height** - The new height of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Texture:SetKeyFocus
Sets the key focus status. Note that only one frame can be the key focus at a time. Focusing on another frame will automatically unset the current focus.
 
    Element:SetKeyFocus(focus)   -- boolean
#### Parameters:
**focus** - The new key focus setting.
### Texture:SetLayer
Sets the frame layer for this frame. This can be any number. Frames are drawn in ascending order. If two frames have the same layer number, then the order of those frames is undefined, but guarantees no Z-fighting. Frames are always drawn after their parents.
 
    Frame:SetLayer(layer)   -- number
#### Parameters:
**layer** - The new layer for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Texture:SetMouseMasking
Sets the frame's mouse masking mode.
 
    Element:SetMouseMasking(mask)   -- string
#### Parameters:
**mask** - The new mouse masking mode. "full" is the standard mode, and means that creating any Left, Right, or movement-related mouse event will cause the frame to accept and consume any event from any of those types. "limited" causes the frame to accept and consume only events for buttons that have been hooked, so that hooking "LeftDown" will still pass Right mouse events through the frame. Note that hooking any mouse event will still consume MouseMove/In/Out events.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Texture:SetMouseoverUnit
Sets the unit that will be represented by this frame.
 
    Frame:SetMouseoverUnit(unit)   -- unit
    Frame:SetMouseoverUnit(unit)   -- nil
#### Parameters:
**unit** - The new mouseover unit. May be a unit ID or a unit specifier. Pass in nil to disable the mouseover effect.
#### Restrictions:
Permitted only on a frame with the "restricted" SecureMode while the addon environment is not secured.
### Texture:SetParent
Sets the parent of this frame. Circular dependencies result in undefined behavior. If this frame's secure mode is "restricted", then its parent must also have a secure mode of "restricted".
 
    Frame:SetParent(parent)   -- Element
#### Parameters:
**parent** - The new parent for this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Texture:SetPoint
Pins a point on this frame to a location on another frame. This is a rather complex function and you should look at examples to see how to use it.
This function can take many different forms. In general, it looks like this: SetPoint(point_on_this_frame, target_frame, point_on_target_frame [, x_offset, y_offset]).
The first part is the point on this frame that will be attached. Usually, these are string identifiers. "TOPLEFT", "TOPCENTER", "TOPRIGHT", "CENTERLEFT", "CENTER", "CENTERRIGHT", "BOTTOMLEFT", "BOTTOMCENTER", "BOTTOMRIGHT". You may also use a string identifier that refers to a single axis - "TOP", "BOTTOM", "LEFT", "RIGHT", "CENTERX", "CENTERY". If you want more direct numeric control you can use number pairs. 0,0 is equivalent to "TOPLEFT", 1,1 is equivalent to "BOTTOMRIGHT", 0.5,nil is equivalent to "CENTERX".
The second part is the frame to attach to, as well as the point on that frame to attach to. The frame is simply passing in the frame table. The point is the same identifier or number pair as the first parameter.
Optionally, you may include an X or Y offset to the point.
This connection is permanent, and if the target frame moves, this frame will move along with it. 
Caveat: If the target is a frame set to the "restricted" SecureMode, and the client is currently in "secure" mode, then unexpected behavior may occur.
 
    Frame:SetPoint(...)   -- ...
#### Parameters:
**...** - This function's parameters are complicated. Read the above summary for details.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Texture:SetSecureMode
Sets the frame's secure mode.
"normal" is the standard mode. It allows for most functionality to be used and does not restrict frames in combat. "restricted" allows for certain sensitive functions to be called, but disables a significant amount of functionality in combat. In order to change a frame to "restricted", its parent must already be "restricted". Note that a "restricted" frame can still have "normal" children.
If you are not planning to use any restricted functions, your frame should remain in normal mode.
At the moment, it is not possible to change from "restricted" back to "normal".
 
    Frame:SetSecureMode(secure)   -- string
#### Parameters:
**secure** - The new secure mode. Valid inputs are "normal" and "restricted".
### Texture:SetStrata
Sets the strata for this frame.
 
    Frame:SetStrata(strata)   -- string
#### Parameters:
**strata** - The item's new strata. Must be one of the elements returned by :GetStrataList().
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Texture:SetTexture
Sets the current texture used for this element.
 
    Texture:SetTexture(source, texture)   -- string, string
#### Parameters:
**source** - The source of the resource. "Rift" will take the resource from Rift's internal data. Anything else will take the resource from the addon with that identifier.
**texture** - The actual texture identifier. Either a resource identifier or a filename.
### Texture:SetVisible
Sets the frame's visibility flag. If set to false, then this frame and all its children will not be rendered or accept mouse input.
 
    Element:SetVisible(visible)   -- boolean
#### Parameters:
**visible** - The new visibility flag.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
### Texture:SetWidth
Sets the width of this frame. Undefined results if the frame already has two pinned X coordinates.
 
    Frame:SetWidth(width)   -- number
#### Parameters:
**width** - The new width of this frame.
#### Restrictions:
Not permitted on a frame with the "restricted" SecureMode while the addon environment is secured.
## Native UI
### UI.Native.Ability
The Ability dialog.
### UI.Native.Accolade
The Warfront accolade HUD.
### UI.Native.Achievement
The Achievement dialog.
### UI.Native.AchievementPopup
The "Achievement Complete" popup.
### UI.Native.Adventure
The Instant Adventure dialog.
### UI.Native.Ascend
The Ascend-a-Friend dialog.
### UI.Native.Attunement
The Planar Attunement dialog.
### UI.Native.Auction
The Auction dialog.
### UI.Native.Bag
The bag and currency HUD.
### UI.Native.BagBank1
The first bank inventory bag.
### UI.Native.BagBank2
The second bank inventory bag.
### UI.Native.BagBank3
The third bank inventory bag.
### UI.Native.BagBank4
The fourth bank inventory bag.
### UI.Native.BagBank5
The fifth bank inventory bag.
### UI.Native.BagBank6
The sixth bank inventory bag.
### UI.Native.BagBank7
The seventh bank inventory bag.
### UI.Native.BagBank8
The eighth bank inventory bag.
### UI.Native.BagInventory1
The first main inventory bag.
### UI.Native.BagInventory2
The second main inventory bag.
### UI.Native.BagInventory3
The third main inventory bag.
### UI.Native.BagInventory4
The fourth main inventory bag.
### UI.Native.BagInventory5
The fifth main inventory bag.
### UI.Native.BagInventory6
The sixth main inventory bag.
### UI.Native.BagInventory7
The seventh main inventory bag.
### UI.Native.BagInventory8
The eighth main inventory bag.
### UI.Native.Bank
The Bank dialog.
### UI.Native.BankGuild
The Guild Bank dialog.
### UI.Native.BarBottom1
The first extra bottom action bar.
### UI.Native.BarBottom2
The second extra bottom action bar.
### UI.Native.BarBottom3
The third extra bottom action bar.
### UI.Native.BarBottom4
The fourth extra bottom action bar.
### UI.Native.BarBottom5
The fifth extra bottom action bar.
### UI.Native.BarBottom6
The sixth extra bottom action bar.
### UI.Native.BarMain
The main action bar.
### UI.Native.BarPet
The pet action bar.
### UI.Native.BarSide1
The first extra side action bar.
### UI.Native.BarSide2
The second extra side action bar.
### UI.Native.BarSide3
The third extra side action bar.
### UI.Native.BarSide4
The fourth extra side action bar.
### UI.Native.BarTemporary
The action bar used for temporary abilities.
### UI.Native.Breath
The breath/fatigue bar.
### UI.Native.Buffbar
The player's buff bar.
### UI.Native.Castbar
The player's cast bar.
### UI.Native.Character
The Character dialog.
### UI.Native.Chronicle
The Chronicles dialog.
### UI.Native.Coinlock
The Coin Lock button.
### UI.Native.Console1
The first console window.
### UI.Native.Console2
The second console window.
### UI.Native.Console3
The third console window.
### UI.Native.Console4
The fourth console window.
### UI.Native.Console5
The fifth console window.
### UI.Native.Console6
The sixth console window.
### UI.Native.Console7
The seventh console window.
### UI.Native.ConsoleSetting
The console settings dialog.
### UI.Native.Crafting
The Crafting dialog.
### UI.Native.Ctf
The Capture-the-Flag status indicator.
### UI.Native.Guest
The guest invite dialog for weddings.
### UI.Native.Guild
The Guild dialog.
### UI.Native.GuildCharter
The Guild Charter dialog.
### UI.Native.GuildFinder
The Guild Finder dialog.
### UI.Native.Import
The Import dialog, as reached from the escape menu.
### UI.Native.Keybind
The Keybind dialog.
### UI.Native.Layout
The UI Layout dialog.
### UI.Native.Leaderboard
The Leaderboard dialog.
### UI.Native.Lfg
The Looking-for-Group dialog.
### UI.Native.Loot
The Loot dialog.
### UI.Native.Macro
The Macro dialog.
### UI.Native.MacroIcon
The Macro icons list.
### UI.Native.MacroSlash
The Macro slash commands list.
### UI.Native.Mail
The Mail dialog.
### UI.Native.MailRead
The currently-being-read Mail dialog.
### UI.Native.MapMain
The main map.
### UI.Native.MapMini
The minimap.
### UI.Native.MechanicPlayer
The player-focused class-specific-mechanic HUD element.
### UI.Native.MechanicTarget
The target-focused class-specific-mechanic HUD element.
### UI.Native.Mentor
The Mentoring popup.
### UI.Native.Menu
The main menu HUD element.
### UI.Native.MessageEvent
The event-related message popup.
### UI.Native.MessageStandard
The generic message popup.
### UI.Native.MessageText
The generic text popup.
### UI.Native.MessageWarfront
The warfront-related message popup.
### UI.Native.MessageZone
The zone-related message popup.
### UI.Native.Notification
The notification message popup.
### UI.Native.Notify
The public-group/return-to-graveyard popup button.
### UI.Native.PortraitFocus
The portrait of your focus target.
### UI.Native.PortraitParty1
The portrait of your first partymember.
### UI.Native.PortraitParty1Pet
The portrait of your first partymember's pet.
### UI.Native.PortraitParty2
The portrait of your second partymember.
### UI.Native.PortraitParty2Pet
The portrait of your second partymember's pet.
### UI.Native.PortraitParty3
The portrait of your third partymember.
### UI.Native.PortraitParty3Pet
The portrait of your third partymember's pet.
### UI.Native.PortraitParty4
The portrait of your fourth partymember.
### UI.Native.PortraitParty4Pet
The portrait of your fourth partymember's pet.
### UI.Native.PortraitPet
The portrait of your pet.
### UI.Native.PortraitPlayer
Your portrait.
### UI.Native.PortraitTarget
The portrait of your target.
### UI.Native.PortraitTargetTarget
The portrait of your target's target.
### UI.Native.Quest
The Quest dialog.
### UI.Native.Question
The general-purpose question popup.
### UI.Native.QuestStickies
The Quest sticky HUD.
### UI.Native.Raid
The Raid dialog.
### UI.Native.RaidGroup1
The raid frame for your raid's first group.
### UI.Native.RaidGroup1Pet
The raid frame for your raid's first group's pets.
### UI.Native.RaidGroup2
The raid frame for your raid's second group.
### UI.Native.RaidGroup2Pet
The raid frame for your raid's second group's pets.
### UI.Native.RaidGroup3
The raid frame for your raid's third group.
### UI.Native.RaidGroup3Pet
The raid frame for your raid's third group's pets.
### UI.Native.RaidGroup4
The raid frame for your raid's fourth group.
### UI.Native.RaidGroup4Pet
The raid frame for your raid's fourth group's pets.
### UI.Native.RaidParty
The raid frame used for displaying your party as a raid.
### UI.Native.RaidPartyPet
The raid frame used for displaying your party's pets as a raid.
### UI.Native.Reactive
The reactive-ability popup.
### UI.Native.Recall
The set-recall-point dialog.
### UI.Native.Respec
The respec dialog.
### UI.Native.Rift
The Rift meter HUD.
### UI.Native.Roll1
The first loot-roll popup.
### UI.Native.Roll2
The second loot-roll popup.
### UI.Native.Roll3
The third loot-roll popup.
### UI.Native.Roll4
The fourth loot-roll popup.
### UI.Native.Setting
The Settings dialog.
### UI.Native.Social
The Social dialog.
### UI.Native.Soul
The Soul Tree dialog.
### UI.Native.Split
The stack split popup.
### UI.Native.Streaming
The icon that indicates your current streaming download status.
### UI.Native.Ticket
The Customer Service Ticket dialog.
### UI.Native.Tip
The Game Tip dialog.
### UI.Native.TipAlert
The Game Tip notification popup.
### UI.Native.Tooltip
The tooltip.
### UI.Native.TooltipAnchor
The location that tooltips are anchored to by default.
### UI.Native.Trade
The Trade dialog.
### UI.Native.Tray
The icon tray used for the clock, mail notification, and similar elements.
### UI.Native.TraySocial
The icon tray used for social notifications.
### UI.Native.Treasure
The Treasure popup used to claim dungeon loot.
### UI.Native.Trial
The Trial dialog.
### UI.Native.Upgrade
The Upgrade portrait, used for targets that are upgradeable.
### UI.Native.Warfront
The Warfront dialog.
### UI.Native.WarfrontLeaderboard
The Warfront Leaderboard dialog.
### UI.Native.World
The World Event dialog.
## Events
### Event.Ability.New.Add
Signals the addition of a player ability.
 
    Event.Ability.New.Add(abilities)
#### Parameters:
**abilities** - Lists the abilities that were added. Table from ability ID to "true".
### Event.Ability.New.Cooldown.Begin
Signals the start of an ability's cooldown.
 
    Event.Ability.New.Cooldown.Begin(cooldowns)
#### Parameters:
**cooldowns** - The abilities whose cooldown has been changed. The key is the ability ID, the value is the new cooldown. 0 indicates that the cooldown has finished.
### Event.Ability.New.Cooldown.End
Signals the end of an ability's cooldown. All the values in the "cooldown" parameter will be 0.
 
    Event.Ability.New.Cooldown.End(cooldowns)
#### Parameters:
**cooldowns** - The abilities whose cooldown has been changed. The key is the ability ID, the value is the new cooldown. 0 indicates that the cooldown has finished.
### Event.Ability.New.Range.False
Signals a player ability exiting range from its current target.
 
    Event.Ability.New.Range.False(abilities)
#### Parameters:
**abilities** - The abilities that have entered or exited range. The key is the ability ID, the value is whether they are currently in range.
### Event.Ability.New.Range.True
Signals a player ability entering range from its current target.
 
    Event.Ability.New.Range.True(abilities)
#### Parameters:
**abilities** - The abilities that have entered or exited range. The key is the ability ID, the value is whether they are currently in range.
### Event.Ability.New.Remove
Signals the removal of a player ability.
 
    Event.Ability.New.Remove(abilities)
#### Parameters:
**abilities** - Lists the abilities that were removed. Table from ability ID to "false".
### Event.Ability.New.Target
Signals a player ability changing its current target.
 
    Event.Ability.New.Target(abilities)
#### Parameters:
**abilities** - The abilities whose target has changed. The key is the ability ID, the value is the new target.
### Event.Ability.New.Usable.False
Signals a player ability becoming unusable.
 
    Event.Ability.New.Usable.False(abilities)
#### Parameters:
**abilities** - The abilities whose usability has changed. The key is the ability ID, the value is the new usability.
### Event.Ability.New.Usable.True
Signals a player ability becoming usable.
 
    Event.Ability.New.Usable.True(abilities)
#### Parameters:
**abilities** - The abilities whose usability has changed. The key is the ability ID, the value is the new usability.
### Event.Achievement.Complete
Signals an achievement completing.
 
    Event.Achievement.Complete(achievement)
#### Parameters:
**achievement** - The ID of the achievement.
### Event.Achievement.Update
Signals a change in an achievement's information.
 
    Event.Achievement.Update(achievements)
#### Parameters:
**achievements** - A table of the achievements, in {id = true} format.
### Event.Addon.Load.Begin
Signals the beginning of an addon's loading sequence.
 
    Event.Addon.Load.Begin(addonidentifier)
#### Parameters:
**addonidentifier** - The addon's identifier.
### Event.Addon.Load.End
Signals the end of an addon's loading sequence. At this point, that addon is fully loaded and initialized.
 
    Event.Addon.Load.End(addonidentifier)
#### Parameters:
**addonidentifier** - The addon's identifier.
### Event.Addon.SavedVariables.Load.Begin
Signals the beginning of an addon's saved variable loading.
 
    Event.Addon.SavedVariables.Load.Begin(addonidentifier)
#### Parameters:
**addonidentifier** - The addon's identifier.
### Event.Addon.SavedVariables.Load.End
Signals the end of an addon's saved variable load.
 
    Event.Addon.SavedVariables.Load.End(addonidentifier)
#### Parameters:
**addonidentifier** - The addon's identifier.
### Event.Addon.SavedVariables.Save.Begin
Signals the beginning of an addon's saved variable saving.
 
    Event.Addon.SavedVariables.Save.Begin(addonidentifier)
#### Parameters:
**addonidentifier** - The addon's identifier.
### Event.Addon.SavedVariables.Save.End
Signals the end of an addon's saved variable saving.
 
    Event.Addon.SavedVariables.Save.End(addonidentifier)
#### Parameters:
**addonidentifier** - The addon's identifier.
### Event.Addon.Shutdown.Begin
Signals the beginning of the shutdown sequence.
 
    Event.Addon.Shutdown.Begin()
### Event.Addon.Shutdown.End
Signals the end of the shutdown sequence. This is the last event that will be sent.
 
    Event.Addon.Shutdown.End()
### Event.Addon.Startup.End
Signals the end of the startup sequence. At this point, all addons are fully loaded and initialized.
 
    Event.Addon.Startup.End()
### Event.Attunement.Progress.Accumulated
Signals your accumulated attunement experience changing.
 
    Event.Attunement.Progress.Accumulated(accumulated)
#### Parameters:
**accumulated** - Total quantity of accumulated attunement experience in this level.
### Event.Attunement.Progress.Available
Signals your available attunement points changing.
 
    Event.Attunement.Progress.Available(available)
#### Parameters:
**available** - Number of unused attunement ranks.
### Event.Attunement.Progress.Rested
Signals your available attunement rested experience changing.
 
    Event.Attunement.Progress.Rested(rested)
#### Parameters:
**rested** - Quantity of available attunement rested experience.
### Event.Attunement.Progress.Spent
Signals your spent attunement points changing.
 
    Event.Attunement.Progress.Spent(spent)
#### Parameters:
**spent** - Number of spent attunement ranks.
### Event.Auction.Scan
Signals incoming auction data.
 
    Event.Auction.Scan(type, auctions)
#### Parameters:
**type** - A table containing information on this scan. In the same format as the parameter to Command.Auction.Scan().
**auctions** - A table of the auctions returned from this scan.
### Event.Auction.Statistics
Signals receipt of auction statistics.
 
    Event.Auction.Statistics(itemtype, data)
#### Parameters:
**itemtype** - Item type that the attached statistics refer to.
**data** - Table containing a series of tables with different timestamps, each containing data for a single day.
#### Returned Members:
**priceVariance** - Variance in item price during this day.
**priceMin** - Minimum item price during this day.
**priceMax** - Maximum item price during this day.
**priceAverage** - Average item price during this day.
**volume** - Quantity of items traded during this day.
**time** - UNIX timestamp of the day.
### Event.Buff.Add
Signals new buffs on a unit.
 
    Event.Buff.Add(unit, buffs)
#### Parameters:
**unit** - The Unit ID of the unit involved.
**buffs** - A table containing the buffs involved. The key is the buff ID, the value is the buff type ID or 'true' if the buff has no type.
### Event.Buff.Change
Signals a change in existing buffs on a unit.
 
    Event.Buff.Change(unit, buffs)
#### Parameters:
**unit** - The Unit ID of the unit involved.
**buffs** - A table containing the buffs involved.
### Event.Buff.Description
Signals a change in an existing buff's detailed description. Value of the key is "true" if detail is now available, "false" if it is no longer available.
 
    Event.Buff.Description(unit, buffs)
#### Parameters:
**unit** - The Unit ID of the unit involved.
**buffs** - A table containing the buffs involved.
### Event.Buff.Remove
Signals removal of buffs from a unit.
 
    Event.Buff.Remove(unit, buffs)
#### Parameters:
**unit** - The Unit ID of the unit involved.
**buffs** - A table containing the buffs involved.
### Event.Chat.Notify
Signals a screen notification. This is generally used as a warning mechanism during boss fights.
 
    Event.Chat.Notify(info)
#### Parameters:
**info** - Detailed information table about this event, containing several named parameters.
#### Returned Members:
**message** - The text said.
### Event.Chat.Npc
Signals an NPC speaking a line of text.
 
    Event.Chat.Npc(info)
#### Parameters:
**info** - Detailed information table about this event, containing several named parameters.
#### Returned Members:
**from** - The unit ID of the speaker, if available.
**message** - The text said.
**fromName** - The name of the speaker.
### Event.Combat.Damage
Signals damage done to a unit. All units referenced by this event will be accessible within this event's handlers.
 
    Event.Combat.Damage(info)
#### Parameters:
**info** - Detailed information table about this event, containing several named parameters.
#### Returned Members:
**crit** - Whether this was the result of a critical hit.
**caster** - The unit ID for this event's initiator, if one exists.
**type** - The damage type. Values include "life", "death", "air", "earth", "fire", "water".
**damageBlocked** - The amount of damage blocked.
**overkill** - The amount of overkill done.
**targetName** - The name of the target, if available.
**casterName** - The name of this event's initiator, if available.
**target** - The unit ID for the target.
**damageIntercepted** - The amount of damage intercepted.
**damageAbsorbed** - The amount of damage absorbed.
**damageModified** - The amount of damage modified.
**damage** - The amount of damage actually done.
**abilityName** - The name of the ability used.
**ability** - The ability ID for the ability used, if available.
### Event.Combat.Death
Signals the death of a unit. All units referenced by this event will be accessible within this event's handlers.
 
    Event.Combat.Death(info)
#### Parameters:
**info** - Detailed information table about this event, containing several named parameters.
#### Returned Members:
**casterName** - The name of this event's initiator, if available.
**target** - The unit ID for the target.
**caster** - The unit ID for this event's initiator, if one exists.
**targetName** - The name of the target, if available.
### Event.Combat.Dodge
Signals a unit dodging an ability. All units referenced by this event will be accessible within this event's handlers.
 
    Event.Combat.Dodge(info)
#### Parameters:
**info** - Detailed information table about this event, containing several named parameters.
#### Returned Members:
**casterName** - The name of this event's initiator, if available.
**target** - The unit ID for the target.
**caster** - The unit ID for this event's initiator, if one exists.
**targetName** - The name of the target, if available.
**abilityName** - The name of the ability used.
**ability** - The ability ID for the ability used, if available.
### Event.Combat.Heal
Signals healing done to a unit. All units referenced by this event will be accessible within this event's handlers.
 
    Event.Combat.Heal(info)
#### Parameters:
**info** - Detailed information table about this event, containing several named parameters.
#### Returned Members:
**crit** - Whether this was the result of a critical hit.
**caster** - The unit ID for this event's initiator, if one exists.
**heal** - The amount healed.
**targetName** - The name of the target, if available.
**casterName** - The name of this event's initiator, if available.
**target** - The unit ID for the target.
**overheal** - The amount of healing past maximum health wasted.
**abilityName** - The name of the ability used.
**ability** - The ability ID for the ability used, if available.
### Event.Combat.Immune
Signals a unit resisting an ability through immunity. All units referenced by this event will be accessible within this event's handlers.
 
    Event.Combat.Immune(info)
#### Parameters:
**info** - Detailed information table about this event, containing several named parameters.
#### Returned Members:
**casterName** - The name of this event's initiator, if available.
**target** - The unit ID for the target.
**caster** - The unit ID for this event's initiator, if one exists.
**targetName** - The name of the target, if available.
**abilityName** - The name of the ability used.
**ability** - The ability ID for the ability used, if available.
### Event.Combat.Miss
Signals an ability's effect missing a unit. All units referenced by this event will be accessible within this event's handlers.
 
    Event.Combat.Miss(info)
#### Parameters:
**info** - Detailed information table about this event, containing several named parameters.
#### Returned Members:
**casterName** - The name of this event's initiator, if available.
**target** - The unit ID for the target.
**caster** - The unit ID for this event's initiator, if one exists.
**targetName** - The name of the target, if available.
**abilityName** - The name of the ability used.
**ability** - The ability ID for the ability used, if available.
### Event.Combat.Parry
Signals a unit parrying an ability. All units referenced by this event will be accessible within this event's handlers.
 
    Event.Combat.Parry(info)
#### Parameters:
**info** - Detailed information table about this event, containing several named parameters.
#### Returned Members:
**casterName** - The name of this event's initiator, if available.
**target** - The unit ID for the target.
**caster** - The unit ID for this event's initiator, if one exists.
**targetName** - The name of the target, if available.
**abilityName** - The name of the ability used.
**ability** - The ability ID for the ability used, if available.
#### Restrictions:
This function is deprecated and will be removed in the future. It should not be used.
### Event.Combat.Resist
Signals a unit resisting an ability. All units referenced by this event will be accessible within this event's handlers.
 
    Event.Combat.Resist(info)
#### Parameters:
**info** - Detailed information table about this event, containing several named parameters.
#### Returned Members:
**casterName** - The name of this event's initiator, if available.
**target** - The unit ID for the target.
**caster** - The unit ID for this event's initiator, if one exists.
**targetName** - The name of the target, if available.
**abilityName** - The name of the ability used.
**ability** - The ability ID for the ability used, if available.
### Event.Currency
Signals a change in the player's available currency.
 
    Event.Currency(currencies)
#### Parameters:
**currencies** - New currency counts, in key/value form.
### Event.Cursor
Signals that the cursor has changed.
 
    Event.Cursor(type, held)
#### Parameters:
**type** - The current cursor type. Valid values include "ability", "item", and "itemtype".
**held** - The blob describing the new element held. Generally, some kind of identifier used in another part of the addon system.
### Event.Dimension.Layout.Add
Signals that a dimension item has been added.
 
    Event.Dimension.Layout.Add(addedItem)
#### Parameters:
**addedItem** - The dimension item that has been added.
### Event.Dimension.Layout.Remove
Signals that a dimension item has been removed.
 
    Event.Dimension.Layout.Remove(removedItem)
#### Parameters:
**removedItem** - The dimension item that has been removed.
### Event.Dimension.Layout.Update
Signals that a dimension item has been updated.
 
    Event.Dimension.Layout.Update(updatedItem)
#### Parameters:
**updatedItem** - The dimension item that has been updated.
### Event.Experience.Accumulated
Signals your accumulated experience changing.
 
    Event.Experience.Accumulated(accumulated)
#### Parameters:
**accumulated** - Total quantity of accumulated experience in this level.
### Event.Experience.Rested
Signals your available rested experience changing.
 
    Event.Experience.Rested(rested)
#### Parameters:
**rested** - Quantity of available rested experience.
### Event.Faction.Notoriety
Signals a change in the player's faction notoriety.
 
    Event.Faction.Notoriety(notoriety)
#### Parameters:
**notoriety** - New notoriety values, in key/value form.
### Event.Guild.Bank.Change
Signals a change in a guild bank vault's information.
 
    Event.Guild.Bank.Change(vaults)
#### Parameters:
**vaults** - A lookup table of vaults that have changed.
### Event.Guild.Bank.Coin
Signals a change in the guild bank's money.
 
    Event.Guild.Bank.Coin(coin)
#### Parameters:
**coin** - The new amount of money in the guild bank.
### Event.Guild.Log
Signals incoming guild log data. Can be triggered manually with Command.Guild.Log.Request().
 
    Event.Guild.Log(log)
#### Parameters:
**log** - An array containing the refreshed guild log items, in order.
#### Returned Members:
**level** - The level involved.
**coin** - The amount of silver involved.
**rank** - The rank involved.
**achievement** - The achievement involved.
**target** - The target of the log event.
**source** - The source of the log event.
**type** - The type of the log message. May be any of achievement, bankCoinDeposit, bankCoinHeal, bankCoinTithe, bankCoinWithdraw, bankItemDeposit, bankItemWithdraw, bankVault, charterSign, charterUnsign, demote, demoteTrial, dimensionBuy, finder, firstAchievement, firstCollection, firstItem, firstKill, firstQuest, form, found, invite, kick, leave, level, motd, promote, rankEdit, rename, transferred, unlearnActive, unlearnPassive, wallDelete, wallModify, xp, or xpQuest.
**count** - The number of items involved.
**xp** - The amount of XP involved.
**item** - The item type involved.
### Event.Guild.Motd
Signals a change in the guild Message of the Day.
 
    Event.Guild.Motd(motd)
#### Parameters:
**motd** - The new Message of the Day.
### Event.Guild.Rank
Signals a change in one of your guild's ranks.
 
    Event.Guild.Rank(ranks)
#### Parameters:
**ranks** - A lookup table of ranks that have changed.
### Event.Guild.Roster.Add
Signals a new player added to the guild roster.
 
    Event.Guild.Roster.Add(add)
#### Parameters:
**add** - The name of the added guildmember.
### Event.Guild.Roster.Detail.Level
Signals a change in a guildmember's level.
 
    Event.Guild.Roster.Detail.Level(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Guild.Roster.Detail.Note
Signals a change in a guildmember's note.
 
    Event.Guild.Roster.Detail.Note(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Guild.Roster.Detail.NoteOfficer
Signals a change in a guildmember's officer note.
 
    Event.Guild.Roster.Detail.NoteOfficer(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Guild.Roster.Detail.Rank
Signals a change in a guildmember's rank.
 
    Event.Guild.Roster.Detail.Rank(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Guild.Roster.Detail.Status
Signals a change in a guildmember's status.
 
    Event.Guild.Roster.Detail.Status(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Guild.Roster.Detail.Zone
Signals a change in a guildmember's zone.
 
    Event.Guild.Roster.Detail.Zone(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Guild.Roster.Remove
Signals a player removed from the guild roster.
 
    Event.Guild.Roster.Remove(remove)
#### Parameters:
**remove** - The name of the removed guildmember.
### Event.Guild.Wall
Signals incoming guild wall data. Can be triggered manually with Command.Guild.Wall.Request().
 
    Event.Guild.Wall(wall)
#### Parameters:
**wall** - An array containing the refreshed guild wall items, in order.
#### Returned Members:
**text** - The text of the guild wall entry.
**time** - The time the wall entry was posted, in UNIX time.
**poster** - The poster of the guild wall entry.
**id** - The ID of the guild wall entry.
### Event.Interaction
Signals a change in available interaction types.
 
    Event.Interaction(interaction, state)
#### Parameters:
**interaction** - The identifier of the interaction type. May be any of "auction", "bank", "guildbank", or "mail".
**state** - Boolean indicating whether or not that interaction is available.
### Event.Item.Slot
Signals that the contents of an item slot have changed.
 
    Event.Item.Slot(updates)
#### Parameters:
**updates** - Table of changes. Key is the slot identifier, value is an item ID, false if the slot is now empty, or the string "nil" if the slot no longer exists.
### Event.Item.Update
Signals that an item has changed.
 
    Event.Item.Update(updates)
#### Parameters:
**updates** - Table of changes. Key is the slot identifier, value is an item ID, false if the slot is now empty, or the string "nil" if the slot no longer exists.
### Event.Mail
Signals a change in the available mail messages.
 
    Event.Mail(mail)
#### Parameters:
**mail** - The changed mail messages. Takes the form of a table. The key is the mail ID, the value is "basic" to indicate basic information available about a piece of mail, "detail" to indicate detailed information available, or false to indicate no information available.
### Event.Map.Add
Signals the addition of a map element.
 
    Event.Map.Add(elements)
#### Parameters:
**elements** - The changed elements. Takes the form of a table. The key is the element ID.
### Event.Map.Change
Signals the change of a map element, in some manner besides coordinates.
 
    Event.Map.Change(elements)
#### Parameters:
**elements** - The changed elements. Takes the form of a table. The key is the element ID.
### Event.Map.Detail.Coord
Signals the coordinate change of a map element.
 
    Event.Map.Detail.Coord(x, y, z)
#### Parameters:
**x** - The new X coordinate of the changed elements. Takes the form of a table. The key is the element ID.
**y** - The new Y coordinate of the changed elements. Takes the form of a table. The key is the element ID.
**z** - The new Z coordinate of the changed elements. Takes the form of a table. The key is the element ID.
### Event.Map.Remove
Signals the removal of a map element.
 
    Event.Map.Remove(elements)
#### Parameters:
**elements** - The changed elements. Takes the form of a table. The key is the element ID.
### Event.Map.Waypoint.Update
Signals the change of a player's waypoint.
 
    Event.Map.Waypoint.Update(units)
#### Parameters:
**units** - The unit whose waypoint has changed. Takes the form of a table. The key is the element ID.
### Event.Message.Receive
Signals the receipt of an addon message.
 
    Event.Message.Receive(from, type, channel, identifier, data)
#### Parameters:
**from** - The name of the player this message is from.
**type** - The type of this message. May be any of "send", "tell", "channel", "guild", "officer", "party", "raid", "say", or "yell".
**channel** - If type is "channel", the channel that this message was sent to. Otherwise, nil.
**identifier** - The identifier for this message.
**data** - The data payload for this message type.
### Event.Minion.Adventure.Change
Signals a change in a minion adventure.
 
    Event.Minion.Adventure.Change(adventures)
#### Parameters:
**adventures** - A table of minion adventures that have changed. Takes the form of a table. The key is the minion adventure ID.
### Event.Minion.Minion.Change
Signals a change in a minion.
 
    Event.Minion.Minion.Change(minions)
#### Parameters:
**minions** - A table of minions that have changed. Takes the form of a table. The key is the minion ID.
### Event.Mouse.Move
Signals the mouse moving.
 
    Event.Mouse.Move(x, y)
#### Parameters:
**x** - The mouse's new X position.
**y** - The mouse's new Y position.
### Event.Pvp.History.Favor
Signals your historical favor changing.
 
    Event.Pvp.History.Favor(favor)
#### Parameters:
**favor** - Amount of lifetime favor accumulated.
### Event.Pvp.History.Kill.Today
Signals your daily kill count changing.
 
    Event.Pvp.History.Kill.Today(kill)
#### Parameters:
**kill** - Amount of kills made in this time period.
### Event.Pvp.Prestige.Accumulated
Signals your accumulated prestige changing.
 
    Event.Pvp.Prestige.Accumulated(accumulated)
#### Parameters:
**accumulated** - Total quantity of accumulated prestige in this rank.
### Event.Pvp.Prestige.Rank
Signals your prestige rank changing.
 
    Event.Pvp.Prestige.Rank(rank)
#### Parameters:
**rank** - Current prestige rank.
### Event.Quest.Abandon
Signals that a quest has been abandoned.
 
    Event.Quest.Abandon(elements)
#### Parameters:
**elements** - The changed elements. Takes the form of a table. The key is the element ID.
### Event.Quest.Accept
Signals that a quest has been accepted.
 
    Event.Quest.Accept(elements)
#### Parameters:
**elements** - The changed elements. Takes the form of a table. The key is the element ID.
### Event.Quest.Change
Signals that a quest has been changed in some manner, most likely progress in an objective.
 
    Event.Quest.Change(elements)
#### Parameters:
**elements** - The changed elements. Takes the form of a table. The key is the element ID.
### Event.Quest.Complete
Signals that a quest has been completed.
 
    Event.Quest.Complete(elements)
#### Parameters:
**elements** - The changed elements. Takes the form of a table. The key is the element ID.
### Event.Queue.Status
Signals that a queue may have left or entered the throttled state.
 
    Event.Queue.Status(id)
#### Parameters:
**id** - The ID of the queue affected. To get the actual new state, use Inspect.Queue.Status().
### Event.Slash.igor
Addon-created event. No documentation available.
 
    Event.Slash.igor(handle, ...)
#### Parameters:
**handle** - nil
### Event.Slash.x
Addon-created event. No documentation available.
 
    Event.Slash.x(handle, ...)
#### Parameters:
**handle** - nil
### Event.Social.Friend
Signals a change in your friend list.
 
    Event.Social.Friend(friends)
#### Parameters:
**friends** - A lookup table containing the changed friends.
### Event.Social.Ignore
Signals a change in your ignore list.
 
    Event.Social.Ignore(ignores)
#### Parameters:
**ignores** - A lookup table containing the changed ignored players.
### Event.Stat
Signals a change in the player's stats.
 
    Event.Stat(stats)
#### Parameters:
**stats** - A table from stat ID to stat value, containing the stats that changed.
### Event.Storage.Get
Signals receiving storage data from a target.
 
    Event.Storage.Get(target, segment, identifier, read, write, data)
#### Parameters:
**target** - The name of the player that has been inspected.
**segment** - The segment that has been inspected, either "player" or "guild".
**identifier** - The identifier that was inspected.
**read** - The read permissions of this element. See Command.Storage.Set() for details.
**write** - The write permissions of this element. See Command.Storage.Set() for details.
**data** - The data contained in the requested element.
### Event.Storage.List
Signals receiving a list of storage elements from a target.
 
    Event.Storage.List(target, segment, identifiers)
#### Parameters:
**target** - The name of the player that has been inspected.
**segment** - The segment that has been inspected, either "player" or "guild".
**identifiers** - A list of identifiers available to you, in {identifier = checksum} format.
### Event.System.Error
Signals that an addon error has occurred. To prevent infinite loops, errors in handlers for this event may not result in further events being triggered.
 
    Event.System.Error(error)
#### Parameters:
**error** - Table containing error data.
#### Returned Members:
**deprecation** - Indicates that the error was caused by attempted use of deprecated functionality. May appear in error types event, frameEvent, script, dispatch, queue, callback, or fileRun.
**info** - The info string provided as part of the event handler. Used in error types event, dispatch, perfWarning, perfError, and requirement.
**id** - The internal ID of this error.
**file** - The name of the file responsible. Used in error types fileNotFound, fileLoad, and fileRun.
**error** - The actual error message generated. Used in error types event, frameEvent, script, dispatch, fileLoad, fileRun, and text.
**type** - Error type. "event" indicates an error within a global event handler. "frameEvent" indicates an error within a frame event handler. "script" indicates an error within a user-entered /script. "dispatch" indicates an error within a Utility.Dispatch handler. "internal" indicates an error within Rift's code (hopefully you'll never see this). "fileNotFound" indicates a missing file when attempting to load an addon. "fileLoad" indicates a parse failure when attempting to load an addon. "fileRun" indicates an execution error when attempting to load an addon. "layoutLoop" indicates a dependency loop found when evaluating the layout. "layoutError" indicates an invalid position found when evaluating the layout. "perfWarning" indicates a watchdog performance warning. "perfError" indicates a watchdog performance error, and that the Lua thread may have been interrupted. "text" indicates an error in Lua code embedded in HTML text. "requirement" indicates an error caused by not fulfilling function requirements.
**event** - The name of the event responsible. Used in error types event, frameEvent, and internal.
**frame** - The name of the frame that the event was generated on. Used in error type frameEvent.
**axis** - The axis influenced by the error. Used in error types layoutLoop and layoutError.
**script** - The exact script entered by the user. Used in error type script.
**addon** - The identifier of the responsible addon. Used in error types event, frameEvent, dispatch, fileNotFound, fileLoad, fileRun, perfWarning, perfError, text, and requirement.
**stacktrace** - The stacktrace at the point of the error. Used in error types event, frameEvent, script, dispatch, fileRun, layoutLoop, layoutError, perfWarning, perfError, text, and requirement.
### Event.System.Secure.Enter
Signals that the system is entering Secure mode. Usually this is equivalent to entering combat. Functions that may not be called in Secure mode will be locked after this event is complete.
 
    Event.System.Secure.Enter()
### Event.System.Secure.Leave
Signals that the system is leaving Secure mode. Usually this is equivalent to leaving combat. Functions that may not be called in Secure mode are unlocked just before this event fires.
 
    Event.System.Secure.Leave()
### Event.System.Texture
Signals information on Rift UI textures that have been seen.
 
    Event.System.Texture(textures)
#### Parameters:
**textures** - A lookup table of all textures that has been seen since the last event.
### Event.System.Update.Begin
Signals the beginning of a frame render. This is your last chance to make UI changes for this frame.
 
    Event.System.Update.Begin()
### Event.System.Update.End
Signals the end of a frame render.
 
    Event.System.Update.End()
### Event.TEMPORARY.Experience
Signals a change in the player's experience.
 
    Event.TEMPORARY.Experience(accumulated, rested, needed)
#### Parameters:
**accumulated** - The amount of  experience that has accumulated towards the next level.
**rested** - The amount of rested experience that has accumulated.
**needed** - The amount of experience required to reach the next level.
#### Restrictions:
This function is deprecated and will be removed in the future. It should not be used.
### Event.TEMPORARY.Role
Signals a change in the player's current role.
 
    Event.TEMPORARY.Role(role)
#### Parameters:
**role** - The ID of the new role.
#### Restrictions:
This function will be removed in the future.
### Event.Title.Add
Signals the addition of a new title.
 
    Event.Title.Add(titles)
#### Parameters:
**titles** - A lookup table of newly available titles.
### Event.Tooltip
Signals that the tooltip has changed.
 
    Event.Tooltip(type, shown, buff)
#### Parameters:
**type** - The current tooltip type. Valid values include "ability", "buff", "item", "itemtype", and "unit".
**shown** - The blob describing the new element shown. Generally, some kind of identifier used in another part of the addon system.
**buff** - The ID of the tooltip's new buff. "shown" will be a unit in this case.
### Event.UI.Button.Left.Press
Signals that a Button has been pressed with the left mouse button. Equivalent to LeftClick, but triggers only while the button is enabled.
 
    Event.UI.Button.Left.Press(handle)
#### Parameters:
**handle** - nil
### Event.UI.Checkbox.Change
Signals that a checkbox's state has changed.
 
    Event.UI.Checkbox.Change(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Key.Down
Signals that a keyboard key has been pressed. Requires key focus.
 
    Event.UI.Input.Key.Down(handle, key)
#### Parameters:
**handle** - nil
**key** - Key code for the applicable key. Generally, the lowercase non-shifted result of pressing that key. Will be a user-identifiable string for keys without standard display representations.
### Event.UI.Input.Key.Down.Bubble
Event.UI.Input.Key.Down.Bubble@summary
 
    Event.UI.Input.Key.Down.Bubble(handle, key)
#### Parameters:
**handle** - nil
**key** - Key code for the applicable key. Generally, the lowercase non-shifted result of pressing that key. Will be a user-identifiable string for keys without standard display representations.
### Event.UI.Input.Key.Down.Dive
Event.UI.Input.Key.Down.Dive@summary
 
    Event.UI.Input.Key.Down.Dive(handle, key)
#### Parameters:
**handle** - nil
**key** - Key code for the applicable key. Generally, the lowercase non-shifted result of pressing that key. Will be a user-identifiable string for keys without standard display representations.
### Event.UI.Input.Key.Focus.Gain
Signals that a frame has gained the key focus.
 
    Event.UI.Input.Key.Focus.Gain(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Key.Focus.Gain.Bubble
Event.UI.Input.Key.Focus.Gain.Bubble@summary
 
    Event.UI.Input.Key.Focus.Gain.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Key.Focus.Gain.Dive
Event.UI.Input.Key.Focus.Gain.Dive@summary
 
    Event.UI.Input.Key.Focus.Gain.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Key.Focus.Loss
Signals that a frame has lost the key focus.
 
    Event.UI.Input.Key.Focus.Loss(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Key.Focus.Loss.Bubble
Event.UI.Input.Key.Focus.Loss.Bubble@summary
 
    Event.UI.Input.Key.Focus.Loss.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Key.Focus.Loss.Dive
Event.UI.Input.Key.Focus.Loss.Dive@summary
 
    Event.UI.Input.Key.Focus.Loss.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Key.Repeat
Signals that a keyboard key is repeating. Requires key focus.
 
    Event.UI.Input.Key.Repeat(handle, key)
#### Parameters:
**handle** - nil
**key** - Key code for the applicable key. Generally, the lowercase non-shifted result of pressing that key. Will be a user-identifiable string for keys without standard display representations.
### Event.UI.Input.Key.Repeat.Bubble
Event.UI.Input.Key.Repeat.Bubble@summary
 
    Event.UI.Input.Key.Repeat.Bubble(handle, key)
#### Parameters:
**handle** - nil
**key** - Key code for the applicable key. Generally, the lowercase non-shifted result of pressing that key. Will be a user-identifiable string for keys without standard display representations.
### Event.UI.Input.Key.Repeat.Dive
Event.UI.Input.Key.Repeat.Dive@summary
 
    Event.UI.Input.Key.Repeat.Dive(handle, key)
#### Parameters:
**handle** - nil
**key** - Key code for the applicable key. Generally, the lowercase non-shifted result of pressing that key. Will be a user-identifiable string for keys without standard display representations.
### Event.UI.Input.Key.Type
Signals that text has been typed. Requires key focus.
 
    Event.UI.Input.Key.Type(handle, text)
#### Parameters:
**handle** - nil
**text** - The text that has been entered.
### Event.UI.Input.Key.Type.Bubble
Event.UI.Input.Key.Type.Bubble@summary
 
    Event.UI.Input.Key.Type.Bubble(handle, text)
#### Parameters:
**handle** - nil
**text** - The text that has been entered.
### Event.UI.Input.Key.Type.Dive
Event.UI.Input.Key.Type.Dive@summary
 
    Event.UI.Input.Key.Type.Dive(handle, text)
#### Parameters:
**handle** - nil
**text** - The text that has been entered.
### Event.UI.Input.Key.Up
Signals that a keyboard key has been released. Requires key focus.
 
    Event.UI.Input.Key.Up(handle, key)
#### Parameters:
**handle** - nil
**key** - Key code for the applicable key. Generally, the lowercase non-shifted result of pressing that key. Will be a user-identifiable string for keys without standard display representations.
### Event.UI.Input.Key.Up.Bubble
Event.UI.Input.Key.Up.Bubble@summary
 
    Event.UI.Input.Key.Up.Bubble(handle, key)
#### Parameters:
**handle** - nil
**key** - Key code for the applicable key. Generally, the lowercase non-shifted result of pressing that key. Will be a user-identifiable string for keys without standard display representations.
### Event.UI.Input.Key.Up.Dive
Event.UI.Input.Key.Up.Dive@summary
 
    Event.UI.Input.Key.Up.Dive(handle, key)
#### Parameters:
**handle** - nil
**key** - Key code for the applicable key. Generally, the lowercase non-shifted result of pressing that key. Will be a user-identifiable string for keys without standard display representations.
### Event.UI.Input.Mouse.Cursor.In
Signals that the cursor has moved into a frame.
 
    Event.UI.Input.Mouse.Cursor.In(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Cursor.In.Bubble
Event.UI.Input.Mouse.Cursor.In.Bubble@summary
 
    Event.UI.Input.Mouse.Cursor.In.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Cursor.In.Dive
Event.UI.Input.Mouse.Cursor.In.Dive@summary
 
    Event.UI.Input.Mouse.Cursor.In.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Cursor.Move
Signals that the cursor has moved within a frame.
 
    Event.UI.Input.Mouse.Cursor.Move(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Cursor.Move.Bubble
Event.UI.Input.Mouse.Cursor.Move.Bubble@summary
 
    Event.UI.Input.Mouse.Cursor.Move.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Cursor.Move.Dive
Event.UI.Input.Mouse.Cursor.Move.Dive@summary
 
    Event.UI.Input.Mouse.Cursor.Move.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Cursor.Out
Signals that the cursor has moved out of a frame.
 
    Event.UI.Input.Mouse.Cursor.Out(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Cursor.Out.Bubble
Event.UI.Input.Mouse.Cursor.Out.Bubble@summary
 
    Event.UI.Input.Mouse.Cursor.Out.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Cursor.Out.Dive
Event.UI.Input.Mouse.Cursor.Out.Dive@summary
 
    Event.UI.Input.Mouse.Cursor.Out.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Left.Click
Signals that the user has clicked the left mouse button.
 
    Event.UI.Input.Mouse.Left.Click(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Left.Click.Bubble
Event.UI.Input.Mouse.Left.Click.Bubble@summary
 
    Event.UI.Input.Mouse.Left.Click.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Left.Click.Dive
Event.UI.Input.Mouse.Left.Click.Dive@summary
 
    Event.UI.Input.Mouse.Left.Click.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Left.Down
Signals that the user has pressed the left mouse button.
 
    Event.UI.Input.Mouse.Left.Down(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Left.Down.Bubble
Event.UI.Input.Mouse.Left.Down.Bubble@summary
 
    Event.UI.Input.Mouse.Left.Down.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Left.Down.Dive
Event.UI.Input.Mouse.Left.Down.Dive@summary
 
    Event.UI.Input.Mouse.Left.Down.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Left.Up
Signals that the user has released the left mouse button.
 
    Event.UI.Input.Mouse.Left.Up(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Left.Up.Bubble
Event.UI.Input.Mouse.Left.Up.Bubble@summary
 
    Event.UI.Input.Mouse.Left.Up.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Left.Up.Dive
Event.UI.Input.Mouse.Left.Up.Dive@summary
 
    Event.UI.Input.Mouse.Left.Up.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Left.Upoutside
Signals that the user has released the left mouse button off this frame after pressing it on this frame.
 
    Event.UI.Input.Mouse.Left.Upoutside(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Left.Upoutside.Bubble
Event.UI.Input.Mouse.Left.Upoutside.Bubble@summary
 
    Event.UI.Input.Mouse.Left.Upoutside.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Left.Upoutside.Dive
Event.UI.Input.Mouse.Left.Upoutside.Dive@summary
 
    Event.UI.Input.Mouse.Left.Upoutside.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Middle.Click
Signals that the user has clicked the middle mouse button.
 
    Event.UI.Input.Mouse.Middle.Click(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Middle.Click.Bubble
Event.UI.Input.Mouse.Middle.Click.Bubble@summary
 
    Event.UI.Input.Mouse.Middle.Click.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Middle.Click.Dive
Event.UI.Input.Mouse.Middle.Click.Dive@summary
 
    Event.UI.Input.Mouse.Middle.Click.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Middle.Down
Signals that the user has pressed the middle mouse button.
 
    Event.UI.Input.Mouse.Middle.Down(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Middle.Down.Bubble
Event.UI.Input.Mouse.Middle.Down.Bubble@summary
 
    Event.UI.Input.Mouse.Middle.Down.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Middle.Down.Dive
Event.UI.Input.Mouse.Middle.Down.Dive@summary
 
    Event.UI.Input.Mouse.Middle.Down.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Middle.Up
Signals that the user has released the middle mouse button.
 
    Event.UI.Input.Mouse.Middle.Up(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Middle.Up.Bubble
Event.UI.Input.Mouse.Middle.Up.Bubble@summary
 
    Event.UI.Input.Mouse.Middle.Up.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Middle.Up.Dive
Event.UI.Input.Mouse.Middle.Up.Dive@summary
 
    Event.UI.Input.Mouse.Middle.Up.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Middle.Upoutside
Signals that the user has released the middle mouse button off this frame after pressing it on this frame.
 
    Event.UI.Input.Mouse.Middle.Upoutside(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Middle.Upoutside.Bubble
Event.UI.Input.Mouse.Middle.Upoutside.Bubble@summary
 
    Event.UI.Input.Mouse.Middle.Upoutside.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Middle.Upoutside.Dive
Event.UI.Input.Mouse.Middle.Upoutside.Dive@summary
 
    Event.UI.Input.Mouse.Middle.Upoutside.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse4.Click
Signals that the user has clicked the fourth mouse button.
 
    Event.UI.Input.Mouse.Mouse4.Click(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse4.Click.Bubble
Event.UI.Input.Mouse.Mouse4.Click.Bubble@summary
 
    Event.UI.Input.Mouse.Mouse4.Click.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse4.Click.Dive
Event.UI.Input.Mouse.Mouse4.Click.Dive@summary
 
    Event.UI.Input.Mouse.Mouse4.Click.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse4.Down
Signals that the user has pressed the fourth mouse button.
 
    Event.UI.Input.Mouse.Mouse4.Down(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse4.Down.Bubble
Event.UI.Input.Mouse.Mouse4.Down.Bubble@summary
 
    Event.UI.Input.Mouse.Mouse4.Down.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse4.Down.Dive
Event.UI.Input.Mouse.Mouse4.Down.Dive@summary
 
    Event.UI.Input.Mouse.Mouse4.Down.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse4.Up
Signals that the user has released the fourth mouse button.
 
    Event.UI.Input.Mouse.Mouse4.Up(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse4.Up.Bubble
Event.UI.Input.Mouse.Mouse4.Up.Bubble@summary
 
    Event.UI.Input.Mouse.Mouse4.Up.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse4.Up.Dive
Event.UI.Input.Mouse.Mouse4.Up.Dive@summary
 
    Event.UI.Input.Mouse.Mouse4.Up.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse4.Upoutside
Signals that the user has released the fourth mouse button off this frame after pressing it on this frame.
 
    Event.UI.Input.Mouse.Mouse4.Upoutside(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse4.Upoutside.Bubble
Event.UI.Input.Mouse.Mouse4.Upoutside.Bubble@summary
 
    Event.UI.Input.Mouse.Mouse4.Upoutside.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse4.Upoutside.Dive
Event.UI.Input.Mouse.Mouse4.Upoutside.Dive@summary
 
    Event.UI.Input.Mouse.Mouse4.Upoutside.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse5.Click
Signals that the user has clicked the fifth mouse button.
 
    Event.UI.Input.Mouse.Mouse5.Click(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse5.Click.Bubble
Event.UI.Input.Mouse.Mouse5.Click.Bubble@summary
 
    Event.UI.Input.Mouse.Mouse5.Click.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse5.Click.Dive
Event.UI.Input.Mouse.Mouse5.Click.Dive@summary
 
    Event.UI.Input.Mouse.Mouse5.Click.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse5.Down
Signals that the user has pressed the fifth mouse button.
 
    Event.UI.Input.Mouse.Mouse5.Down(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse5.Down.Bubble
Event.UI.Input.Mouse.Mouse5.Down.Bubble@summary
 
    Event.UI.Input.Mouse.Mouse5.Down.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse5.Down.Dive
Event.UI.Input.Mouse.Mouse5.Down.Dive@summary
 
    Event.UI.Input.Mouse.Mouse5.Down.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse5.Up
Signals that the user has released the fifth mouse button.
 
    Event.UI.Input.Mouse.Mouse5.Up(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse5.Up.Bubble
Event.UI.Input.Mouse.Mouse5.Up.Bubble@summary
 
    Event.UI.Input.Mouse.Mouse5.Up.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse5.Up.Dive
Event.UI.Input.Mouse.Mouse5.Up.Dive@summary
 
    Event.UI.Input.Mouse.Mouse5.Up.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse5.Upoutside
Signals that the user has released the fifth mouse button off this frame after pressing it on this frame.
 
    Event.UI.Input.Mouse.Mouse5.Upoutside(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse5.Upoutside.Bubble
Event.UI.Input.Mouse.Mouse5.Upoutside.Bubble@summary
 
    Event.UI.Input.Mouse.Mouse5.Upoutside.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Mouse5.Upoutside.Dive
Event.UI.Input.Mouse.Mouse5.Upoutside.Dive@summary
 
    Event.UI.Input.Mouse.Mouse5.Upoutside.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Right.Click
Signals that the user has clicked the right mouse button.
 
    Event.UI.Input.Mouse.Right.Click(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Right.Click.Bubble
Event.UI.Input.Mouse.Right.Click.Bubble@summary
 
    Event.UI.Input.Mouse.Right.Click.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Right.Click.Dive
Event.UI.Input.Mouse.Right.Click.Dive@summary
 
    Event.UI.Input.Mouse.Right.Click.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Right.Down
Signals that the user has pressed the right mouse button.
 
    Event.UI.Input.Mouse.Right.Down(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Right.Down.Bubble
Event.UI.Input.Mouse.Right.Down.Bubble@summary
 
    Event.UI.Input.Mouse.Right.Down.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Right.Down.Dive
Event.UI.Input.Mouse.Right.Down.Dive@summary
 
    Event.UI.Input.Mouse.Right.Down.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Right.Up
Signals that the user has released the right mouse button.
 
    Event.UI.Input.Mouse.Right.Up(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Right.Up.Bubble
Event.UI.Input.Mouse.Right.Up.Bubble@summary
 
    Event.UI.Input.Mouse.Right.Up.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Right.Up.Dive
Event.UI.Input.Mouse.Right.Up.Dive@summary
 
    Event.UI.Input.Mouse.Right.Up.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Right.Upoutside
Signals that the user has released the right mouse button off this frame after pressing it on this frame.
 
    Event.UI.Input.Mouse.Right.Upoutside(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Right.Upoutside.Bubble
Event.UI.Input.Mouse.Right.Upoutside.Bubble@summary
 
    Event.UI.Input.Mouse.Right.Upoutside.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Right.Upoutside.Dive
Event.UI.Input.Mouse.Right.Upoutside.Dive@summary
 
    Event.UI.Input.Mouse.Right.Upoutside.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Wheel.Back
Signals that the user has moved the mousewheel back.
 
    Event.UI.Input.Mouse.Wheel.Back(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Wheel.Back.Bubble
Event.UI.Input.Mouse.Wheel.Back.Bubble@summary
 
    Event.UI.Input.Mouse.Wheel.Back.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Wheel.Back.Dive
Event.UI.Input.Mouse.Wheel.Back.Dive@summary
 
    Event.UI.Input.Mouse.Wheel.Back.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Wheel.Forward
Signals that the user has moved the mousewheel forwards.
 
    Event.UI.Input.Mouse.Wheel.Forward(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Wheel.Forward.Bubble
Event.UI.Input.Mouse.Wheel.Forward.Bubble@summary
 
    Event.UI.Input.Mouse.Wheel.Forward.Bubble(handle)
#### Parameters:
**handle** - nil
### Event.UI.Input.Mouse.Wheel.Forward.Dive
Event.UI.Input.Mouse.Wheel.Forward.Dive@summary
 
    Event.UI.Input.Mouse.Wheel.Forward.Dive(handle)
#### Parameters:
**handle** - nil
### Event.UI.Layout.Layer
Signals a change in layer.
 
    Event.UI.Layout.Layer(handle)
#### Parameters:
**handle** - nil
### Event.UI.Layout.Move
Signals a change in the top-left corner's position. May be delayed after the actual movement.
 
    Event.UI.Layout.Move(handle)
#### Parameters:
**handle** - nil
### Event.UI.Layout.Size
Signals a change in size. May be delayed after the actual change.
 
    Event.UI.Layout.Size(handle)
#### Parameters:
**handle** - nil
### Event.UI.Layout.Strata
Signals a change in strata.
 
    Event.UI.Layout.Strata(handle)
#### Parameters:
**handle** - nil
### Event.UI.Native.Loaded
Signals a change in this native frame's loaded state.
 
    Event.UI.Native.Loaded(handle)
#### Parameters:
**handle** - nil
### Event.UI.Scrollbar.Change
Signals a change in scrollbar position.
 
    Event.UI.Scrollbar.Change(handle)
#### Parameters:
**handle** - nil
### Event.UI.Scrollbar.Grab
Signals that a scrollbar has been grabbed.
 
    Event.UI.Scrollbar.Grab(handle)
#### Parameters:
**handle** - nil
### Event.UI.Scrollbar.Release
Signals that a scrollbar has been released.
 
    Event.UI.Scrollbar.Release(handle)
#### Parameters:
**handle** - nil
### Event.UI.Slider.Change
Signals a change in slider position.
 
    Event.UI.Slider.Change(handle)
#### Parameters:
**handle** - nil
### Event.UI.Slider.Grab
Signals that a slider has been grabbed.
 
    Event.UI.Slider.Grab(handle)
#### Parameters:
**handle** - nil
### Event.UI.Slider.Release
Signals that a slider has been released.
 
    Event.UI.Slider.Release(handle)
#### Parameters:
**handle** - nil
### Event.UI.Textfield.Change
Signals a change in textfield contents.
 
    Event.UI.Textfield.Change(handle)
#### Parameters:
**handle** - nil
### Event.UI.Textfield.Select
Signals a change in textfield selection.
 
    Event.UI.Textfield.Select(handle)
#### Parameters:
**handle** - nil
### Event.Unit.Add
Signals the addition of units to unit specifiers. Always preceded by Event.Unit.Change.Remove. Triggers for unit IDs that have been removed from the specifier tree, but may not trigger for any children of those units. You may want to use LibUnitChange instead of this event.
 
    Event.Unit.Add(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the unit specifier, or "false" if the unit now has no specifier.
### Event.Unit.Availability.Full
Signals units entering Full availability.
 
    Event.Unit.Availability.Full(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the unit specifier, or "false" if the unit now has no specifier.
### Event.Unit.Availability.None
Signals units leaving availability entirely.
 
    Event.Unit.Availability.None(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the unit specifier, or "false" if the unit now has no specifier.
### Event.Unit.Availability.Partial
Signals units entering Partial availability, possibly on the way to Full or None availability.
 
    Event.Unit.Availability.Partial(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the unit specifier, or "false" if the unit now has no specifier.
### Event.Unit.Castbar
Signals a unit's castbar changing.
 
    Event.Unit.Castbar(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is a castbar visibility flag.
### Event.Unit.Detail.Absorb
Signals a unit's damage absorption changing.
 
    Event.Unit.Detail.Absorb(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Unit.Detail.Afk
Signals a unit's AFK flag changing. Will trigger only for partymembers.
 
    Event.Unit.Detail.Afk(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Unit.Detail.Aggro
Signals a unit's aggro flag changing. Will trigger only for groupmembers.
 
    Event.Unit.Detail.Aggro(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.Blocked
Signals a unit's LOS-blocked flag changing. Will trigger only for groupmembers.
 
    Event.Unit.Detail.Blocked(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.Charge
Signals a unit's charge changing. Will trigger only for the player's unit ID.
 
    Event.Unit.Detail.Charge(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Unit.Detail.ChargeMax
Signals a unit's charge maximum changing. Will trigger only for the player's unit ID.
 
    Event.Unit.Detail.ChargeMax(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Unit.Detail.Combat
Signals a unit's combat status changing.
 
    Event.Unit.Detail.Combat(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Unit.Detail.Combo
Signals a unit's combo points changing. Will trigger only for the player's unit ID.
 
    Event.Unit.Detail.Combo(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Unit.Detail.Coord
Signals a change in the unit's location.
 
    Event.Unit.Detail.Coord(x, y, z)
#### Parameters:
**x** - The new X coordinate of the changed elements. Takes the form of a table. The key is the element ID.
**y** - The new Y coordinate of the changed elements. Takes the form of a table. The key is the element ID.
**z** - The new Z coordinate of the changed elements. Takes the form of a table. The key is the element ID.
### Event.Unit.Detail.Energy
Signals a unit's energy changing.
 
    Event.Unit.Detail.Energy(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.EnergyMax
Signals a unit's energy maximum changing.
 
    Event.Unit.Detail.EnergyMax(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.Guild
Signals a unit's guild changing.
 
    Event.Unit.Detail.Guild(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.Health
Signals a unit's health changing.
 
    Event.Unit.Detail.Health(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.HealthCap
Signals a unit's health cap changing.
 
    Event.Unit.Detail.HealthCap(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.HealthMax
Signals a unit's health maximum changing.
 
    Event.Unit.Detail.HealthMax(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.Level
Signals a unit's level changing.
 
    Event.Unit.Detail.Level(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.LocationName
Signals a unit's location name changing.
 
    Event.Unit.Detail.LocationName(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.Mana
Signals a unit's mana changing.
 
    Event.Unit.Detail.Mana(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.ManaMax
Signals a unit's mana maximum changing.
 
    Event.Unit.Detail.ManaMax(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.Mark
Signals a unit's mark changing.
 
    Event.Unit.Detail.Mark(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.Mentoring
Signals a unit's mentoring status changing.
 
    Event.Unit.Detail.Mentoring(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.Name
Signals a unit's name changing.
 
    Event.Unit.Detail.Name(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Unit.Detail.Offline
Signals a unit's offline flag changing. Will trigger only for partymembers.
 
    Event.Unit.Detail.Offline(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Unit.Detail.Planar
Signals a unit's planar charges changing. Will trigger only for the player or groupmembers.
 
    Event.Unit.Detail.Planar(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Unit.Detail.PlanarMax
Signals a unit's maximum planar charges changing. Will trigger only for the player or groupmembers.
 
    Event.Unit.Detail.PlanarMax(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Unit.Detail.Power
Signals a unit's power changing.
 
    Event.Unit.Detail.Power(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.PublicSize
Signals a unit's public group size or status changing.
 
    Event.Unit.Detail.PublicSize(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.Pvp
Signals a unit's PvP flag changing.
 
    Event.Unit.Detail.Pvp(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Unit.Detail.Radius
Signals a unit's radius changing.
 
    Event.Unit.Detail.Radius(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.Ready
Signals a unit's readycheck status changing.
 
    Event.Unit.Detail.Ready(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or the string "nil" in place of nil.
### Event.Unit.Detail.Role
Signals a unit's role changing. Will trigger only for the player or partymembers.
 
    Event.Unit.Detail.Role(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.Tagged
Signals a unit's tagged status changing.
 
    Event.Unit.Detail.Tagged(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.TitlePrefixId
Signals a unit's title prefix ID changing.
 
    Event.Unit.Detail.TitlePrefixId(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.TitlePrefixName
Signals a unit's localized title prefix changing.
 
    Event.Unit.Detail.TitlePrefixName(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
#### Restrictions:
This function is deprecated and will be removed in the future. It should not be used.
### Event.Unit.Detail.TitleSuffixId
Signals a unit's title suffix ID changing.
 
    Event.Unit.Detail.TitleSuffixId(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Detail.TitleSuffixName
Signals a unit's localized title suffix changing.
 
    Event.Unit.Detail.TitleSuffixName(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
#### Restrictions:
This function is deprecated and will be removed in the future. It should not be used.
### Event.Unit.Detail.Vitality
Signals a unit's vitality changing. Will trigger only for the player or groupmembers.
 
    Event.Unit.Detail.Vitality(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Unit.Detail.Warfront
Signals a unit's warfront flag changing. Will trigger only for partymembers.
 
    Event.Unit.Detail.Warfront(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value.
### Event.Unit.Detail.Zone
Signals a change in the unit's zone.
 
    Event.Unit.Detail.Zone(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the new value, or "false" in place of nil.
### Event.Unit.Remove
Signals the removal of units from unit specifiers. Always followed by Event.Unit.Change.Add. Triggers for unit IDs that have been removed from the specifier tree, but may not trigger for any children of those units. You may want to use LibUnitChange instead of this event.
 
    Event.Unit.Remove(units)
#### Parameters:
**units** - Table of the units changed. Key is the unit ID, value is the unit specifier, or "false" if the unit now has no specifier.
## UI Events
### Canvas.Event:KeyDown
Signals a key pressed.
 
    Frame.Event:KeyDown(button)
#### Parameters:
**button** - The key, in string form.
### Canvas.Event:KeyFocusGain
Signals gaining key focus.
 
    Frame.Event:KeyFocusGain()
### Canvas.Event:KeyFocusLoss
Signals losing key focus.
 
    Frame.Event:KeyFocusLoss()
### Canvas.Event:KeyRepeat
Frame.Event:KeyRepeat@summary
 
    Frame.Event:KeyRepeat(button)
#### Parameters:
**button** - The key, in string form.
### Canvas.Event:KeyType
Signals text typed.
 
    Frame.Event:KeyType(typed)
#### Parameters:
**typed** - The text.
### Canvas.Event:KeyUp
Signals a key released.
 
    Frame.Event:KeyUp(button)
#### Parameters:
**button** - The key, in string form.
### Canvas.Event:Layer
Signals a change in the item's layer.
 
    Frame.Event:Layer()
### Canvas.Event:LeftClick
Signals that the mouse's left button has been clicked inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftClick()
### Canvas.Event:LeftDown
Signals that the mouse's left button has been pressed inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftDown()
### Canvas.Event:LeftUp
Signals that the mouse's left button has been released inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUp()
### Canvas.Event:LeftUpoutside
Signals that the mouse's left button has been released outside the frame, after a "Down" message. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUpoutside()
### Canvas.Event:MiddleClick
Signals that the mouse's middle button has been clicked inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleClick()
### Canvas.Event:MiddleDown
Signals that the mouse's middle button has been pressed inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleDown()
### Canvas.Event:MiddleUp
Signals that the mouse's middle button has been released inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUp()
### Canvas.Event:MiddleUpoutside
Signals that the mouse's middle button has been released outside the frame, after a "Down" message. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUpoutside()
### Canvas.Event:Mouse4Click
Signals that the mouse's fourth button has been clicked inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Click()
### Canvas.Event:Mouse4Down
Signals that the mouse's fourth button has been pressed inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Down()
### Canvas.Event:Mouse4Up
Signals that the mouse's fourth button has been released inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Up()
Farseeker: There has been a prison break at the Flatyard! I'm looking for Bounty Hunters to find the Most Wanted!
### Canvas.Event:Mouse4Upoutside
Signals that the mouse's fourth button has been released outside the frame, after a "Down" message. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Upoutside()
### Canvas.Event:Mouse5Click
Signals that the mouse's fifth button has been clicked inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Click()
### Canvas.Event:Mouse5Down
Signals that the mouse's fifth button has been pressed inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Down()
### Canvas.Event:Mouse5Up
Signals that the mouse's fifth button has been released inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Up()
### Canvas.Event:Mouse5Upoutside
Signals that the mouse's fifth button has been released outside the frame, after a "Down" message. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Upoutside()
### Canvas.Event:MouseIn
Signals that the mouse cursor has been moved onto the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseIn()
### Canvas.Event:MouseMove
Signals that the mouse cursor has been moved within the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseMove(x, y)
#### Parameters:
**x** - X coordinate.
**y** - Y coordinate.
### Canvas.Event:MouseOut
Signals that the mouse cursor has been moved off of the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseOut()
### Canvas.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### Canvas.Event:RightClick
Signals that the mouse's right button has been clicked inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightClick()
### Canvas.Event:RightDown
Signals that the mouse's right button has been pressed inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightDown()
### Canvas.Event:RightUp
Signals that the mouse's right button has been released inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUp()
### Canvas.Event:RightUpoutside
Signals that the mouse's right button has been released outside the frame, after a "Down" message. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUpoutside()
### Canvas.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### Canvas.Event:Strata
Signals a change in the item's strata.
 
    Frame.Event:Strata()
### Canvas.Event:WheelBack
Signals that the mousewheel has been moved backward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelBack()
### Canvas.Event:WheelForward
Signals that the mousewheel has been moved forward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelForward()
### Frame.Event:KeyDown
Signals a key pressed.
 
    Frame.Event:KeyDown(button)
#### Parameters:
**button** - The key, in string form.
### Frame.Event:KeyFocusGain
Signals gaining key focus.
 
    Frame.Event:KeyFocusGain()
### Frame.Event:KeyFocusLoss
Signals losing key focus.
 
    Frame.Event:KeyFocusLoss()
### Frame.Event:KeyRepeat
Frame.Event:KeyRepeat@summary
 
    Frame.Event:KeyRepeat(button)
#### Parameters:
**button** - The key, in string form.
### Frame.Event:KeyType
Signals text typed.
 
    Frame.Event:KeyType(typed)
#### Parameters:
**typed** - The text.
### Frame.Event:KeyUp
Signals a key released.
 
    Frame.Event:KeyUp(button)
#### Parameters:
**button** - The key, in string form.
### Frame.Event:Layer
Signals a change in the item's layer.
 
    Frame.Event:Layer()
### Frame.Event:LeftClick
Signals that the mouse's left button has been clicked inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftClick()
### Frame.Event:LeftDown
Signals that the mouse's left button has been pressed inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftDown()
### Frame.Event:LeftUp
Signals that the mouse's left button has been released inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUp()
### Frame.Event:LeftUpoutside
Signals that the mouse's left button has been released outside the frame, after a "Down" message. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUpoutside()
### Frame.Event:MiddleClick
Signals that the mouse's middle button has been clicked inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleClick()
### Frame.Event:MiddleDown
Signals that the mouse's middle button has been pressed inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleDown()
### Frame.Event:MiddleUp
Signals that the mouse's middle button has been released inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUp()
### Frame.Event:MiddleUpoutside
Signals that the mouse's middle button has been released outside the frame, after a "Down" message. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUpoutside()
### Frame.Event:Mouse4Click
Signals that the mouse's fourth button has been clicked inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Click()
### Frame.Event:Mouse4Down
Signals that the mouse's fourth button has been pressed inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Down()
### Frame.Event:Mouse4Up
Signals that the mouse's fourth button has been released inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Up()
### Frame.Event:Mouse4Upoutside
Signals that the mouse's fourth button has been released outside the frame, after a "Down" message. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Upoutside()
### Frame.Event:Mouse5Click
Signals that the mouse's fifth button has been clicked inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Click()
### Frame.Event:Mouse5Down
Signals that the mouse's fifth button has been pressed inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Down()
### Frame.Event:Mouse5Up
Signals that the mouse's fifth button has been released inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Up()
### Frame.Event:Mouse5Upoutside
Signals that the mouse's fifth button has been released outside the frame, after a "Down" message. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Upoutside()
### Frame.Event:MouseIn
Signals that the mouse cursor has been moved onto the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseIn()
### Frame.Event:MouseMove
Signals that the mouse cursor has been moved within the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseMove(x, y)
#### Parameters:
**x** - X coordinate.
**y** - Y coordinate.
### Frame.Event:MouseOut
Signals that the mouse cursor has been moved off of the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseOut()
### Frame.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### Frame.Event:RightClick
Signals that the mouse's right button has been clicked inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightClick()
### Frame.Event:RightDown
Signals that the mouse's right button has been pressed inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightDown()
### Frame.Event:RightUp
Signals that the mouse's right button has been released inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUp()
### Frame.Event:RightUpoutside
Signals that the mouse's right button has been released outside the frame, after a "Down" message. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUpoutside()
### Frame.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### Frame.Event:Strata
Signals a change in the item's strata.
 
    Frame.Event:Strata()
### Frame.Event:WheelBack
Signals that the mousewheel has been moved backward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelBack()
### Frame.Event:WheelForward
Signals that the mousewheel has been moved forward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelForward()
### Layout.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### Layout.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### Mask.Event:KeyDown
Signals a key pressed.
 
    Frame.Event:KeyDown(button)
#### Parameters:
**button** - The key, in string form.
### Mask.Event:KeyFocusGain
Signals gaining key focus.
 
    Frame.Event:KeyFocusGain()
### Mask.Event:KeyFocusLoss
Signals losing key focus.
 
    Frame.Event:KeyFocusLoss()
### Mask.Event:KeyRepeat
Frame.Event:KeyRepeat@summary
 
    Frame.Event:KeyRepeat(button)
#### Parameters:
**button** - The key, in string form.
### Mask.Event:KeyType
Signals text typed.
 
    Frame.Event:KeyType(typed)
#### Parameters:
**typed** - The text.
### Mask.Event:KeyUp
Signals a key released.
 
    Frame.Event:KeyUp(button)
#### Parameters:
**button** - The key, in string form.
### Mask.Event:Layer
Signals a change in the item's layer.
 
    Frame.Event:Layer()
### Mask.Event:LeftClick
Signals that the mouse's left button has been clicked inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftClick()
### Mask.Event:LeftDown
Signals that the mouse's left button has been pressed inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftDown()
### Mask.Event:LeftUp
Signals that the mouse's left button has been released inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUp()
### Mask.Event:LeftUpoutside
Signals that the mouse's left button has been released outside the frame, after a "Down" message. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUpoutside()
### Mask.Event:MiddleClick
Signals that the mouse's middle button has been clicked inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleClick()
### Mask.Event:MiddleDown
Signals that the mouse's middle button has been pressed inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleDown()
### Mask.Event:MiddleUp
Signals that the mouse's middle button has been released inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUp()
### Mask.Event:MiddleUpoutside
Signals that the mouse's middle button has been released outside the frame, after a "Down" message. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUpoutside()
### Mask.Event:Mouse4Click
Signals that the mouse's fourth button has been clicked inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Click()
### Mask.Event:Mouse4Down
Signals that the mouse's fourth button has been pressed inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Down()
### Mask.Event:Mouse4Up
Signals that the mouse's fourth button has been released inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Up()
### Mask.Event:Mouse4Upoutside
Signals that the mouse's fourth button has been released outside the frame, after a "Down" message. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Upoutside()
### Mask.Event:Mouse5Click
Signals that the mouse's fifth button has been clicked inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Click()
### Mask.Event:Mouse5Down
Signals that the mouse's fifth button has been pressed inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Down()
### Mask.Event:Mouse5Up
Signals that the mouse's fifth button has been released inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Up()
### Mask.Event:Mouse5Upoutside
Signals that the mouse's fifth button has been released outside the frame, after a "Down" message. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Upoutside()
### Mask.Event:MouseIn
Signals that the mouse cursor has been moved onto the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseIn()
### Mask.Event:MouseMove
Signals that the mouse cursor has been moved within the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseMove(x, y)
#### Parameters:
**x** - X coordinate.
**y** - Y coordinate.
### Mask.Event:MouseOut
Signals that the mouse cursor has been moved off of the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseOut()
### Mask.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### Mask.Event:RightClick
Signals that the mouse's right button has been clicked inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightClick()
### Mask.Event:RightDown
Signals that the mouse's right button has been pressed inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightDown()
### Mask.Event:RightUp
Signals that the mouse's right button has been released inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUp()
### Mask.Event:RightUpoutside
Signals that the mouse's right button has been released outside the frame, after a "Down" message. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUpoutside()
### Mask.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### Mask.Event:Strata
Signals a change in the item's strata.
 
    Frame.Event:Strata()
### Mask.Event:WheelBack
Signals that the mousewheel has been moved backward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelBack()
### Mask.Event:WheelForward
Signals that the mousewheel has been moved forward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelForward()
### Native.Event:Layer
Signals a change in the item's layer.
 
    Native.Event:Layer()
### Native.Event:Loaded
Signals a change in the item's loaded status.
 
    Native.Event:Loaded()
### Native.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### Native.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### Native.Event:Strata
Signals a change in the item's strata.
 
    Native.Event:Strata()
### RiftButton.Event:KeyDown
Signals a key pressed.
 
    Frame.Event:KeyDown(button)
#### Parameters:
**button** - The key, in string form.
### RiftButton.Event:KeyFocusGain
Signals gaining key focus.
 
    Frame.Event:KeyFocusGain()
### RiftButton.Event:KeyFocusLoss
Signals losing key focus.
 
    Frame.Event:KeyFocusLoss()
### RiftButton.Event:KeyRepeat
Frame.Event:KeyRepeat@summary
 
    Frame.Event:KeyRepeat(button)
#### Parameters:
**button** - The key, in string form.
### RiftButton.Event:KeyType
Signals text typed.
 
    Frame.Event:KeyType(typed)
#### Parameters:
**typed** - The text.
### RiftButton.Event:KeyUp
Signals a key released.
 
    Frame.Event:KeyUp(button)
#### Parameters:
**button** - The key, in string form.
### RiftButton.Event:Layer
Signals a change in the item's layer.
 
    Frame.Event:Layer()
### RiftButton.Event:LeftClick
Signals that the mouse's left button has been clicked inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftClick()
### RiftButton.Event:LeftDown
Signals that the mouse's left button has been pressed inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftDown()
### RiftButton.Event:LeftPress
Signals that the button has been pressed. Equivalent to LeftClick, but triggers only while the button is enabled.
 
    RiftButton.Event:LeftPress()
### RiftButton.Event:LeftUp
Signals that the mouse's left button has been released inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUp()
### RiftButton.Event:LeftUpoutside
Signals that the mouse's left button has been released outside the frame, after a "Down" message. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUpoutside()
### RiftButton.Event:MiddleClick
Signals that the mouse's middle button has been clicked inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleClick()
### RiftButton.Event:MiddleDown
Signals that the mouse's middle button has been pressed inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleDown()
### RiftButton.Event:MiddleUp
Signals that the mouse's middle button has been released inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUp()
### RiftButton.Event:MiddleUpoutside
Signals that the mouse's middle button has been released outside the frame, after a "Down" message. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUpoutside()
### RiftButton.Event:Mouse4Click
Signals that the mouse's fourth button has been clicked inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Click()
### RiftButton.Event:Mouse4Down
Signals that the mouse's fourth button has been pressed inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Down()
### RiftButton.Event:Mouse4Up
Signals that the mouse's fourth button has been released inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Up()
### RiftButton.Event:Mouse4Upoutside
Signals that the mouse's fourth button has been released outside the frame, after a "Down" message. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Upoutside()
### RiftButton.Event:Mouse5Click
Signals that the mouse's fifth button has been clicked inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Click()
### RiftButton.Event:Mouse5Down
Signals that the mouse's fifth button has been pressed inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Down()
### RiftButton.Event:Mouse5Up
Signals that the mouse's fifth button has been released inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Up()
### RiftButton.Event:Mouse5Upoutside
Signals that the mouse's fifth button has been released outside the frame, after a "Down" message. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Upoutside()
### RiftButton.Event:MouseIn
Signals that the mouse cursor has been moved onto the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseIn()
### RiftButton.Event:MouseMove
Signals that the mouse cursor has been moved within the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseMove(x, y)
#### Parameters:
**x** - X coordinate.
**y** - Y coordinate.
### RiftButton.Event:MouseOut
Signals that the mouse cursor has been moved off of the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseOut()
### RiftButton.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### RiftButton.Event:RightClick
Signals that the mouse's right button has been clicked inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightClick()
### RiftButton.Event:RightDown
Signals that the mouse's right button has been pressed inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightDown()
### RiftButton.Event:RightUp
Signals that the mouse's right button has been released inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUp()
### RiftButton.Event:RightUpoutside
Signals that the mouse's right button has been released outside the frame, after a "Down" message. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUpoutside()
### RiftButton.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### RiftButton.Event:Strata
Signals a change in the item's strata.
 
    Frame.Event:Strata()
### RiftButton.Event:WheelBack
Signals that the mousewheel has been moved backward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelBack()
### RiftButton.Event:WheelForward
Signals that the mousewheel has been moved forward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelForward()
### RiftCheckbox.Event:CheckboxChange
Signals that the checkbox's checked state has changed.
 
    RiftCheckbox.Event:CheckboxChange()
### RiftCheckbox.Event:KeyDown
Signals a key pressed.
 
    Frame.Event:KeyDown(button)
#### Parameters:
**button** - The key, in string form.
### RiftCheckbox.Event:KeyFocusGain
Signals gaining key focus.
 
    Frame.Event:KeyFocusGain()
### RiftCheckbox.Event:KeyFocusLoss
Signals losing key focus.
 
    Frame.Event:KeyFocusLoss()
### RiftCheckbox.Event:KeyRepeat
Frame.Event:KeyRepeat@summary
 
    Frame.Event:KeyRepeat(button)
#### Parameters:
**button** - The key, in string form.
### RiftCheckbox.Event:KeyType
Signals text typed.
 
    Frame.Event:KeyType(typed)
#### Parameters:
**typed** - The text.
### RiftCheckbox.Event:KeyUp
Signals a key released.
 
    Frame.Event:KeyUp(button)
#### Parameters:
**button** - The key, in string form.
### RiftCheckbox.Event:Layer
Signals a change in the item's layer.
 
    Frame.Event:Layer()
### RiftCheckbox.Event:LeftClick
Signals that the mouse's left button has been clicked inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftClick()
### RiftCheckbox.Event:LeftDown
Signals that the mouse's left button has been pressed inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftDown()
### RiftCheckbox.Event:LeftUp
Signals that the mouse's left button has been released inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUp()
### RiftCheckbox.Event:LeftUpoutside
Signals that the mouse's left button has been released outside the frame, after a "Down" message. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUpoutside()
### RiftCheckbox.Event:MiddleClick
Signals that the mouse's middle button has been clicked inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleClick()
### RiftCheckbox.Event:MiddleDown
Signals that the mouse's middle button has been pressed inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleDown()
### RiftCheckbox.Event:MiddleUp
Signals that the mouse's middle button has been released inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUp()
### RiftCheckbox.Event:MiddleUpoutside
Signals that the mouse's middle button has been released outside the frame, after a "Down" message. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUpoutside()
### RiftCheckbox.Event:Mouse4Click
Signals that the mouse's fourth button has been clicked inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Click()
### RiftCheckbox.Event:Mouse4Down
Signals that the mouse's fourth button has been pressed inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Down()
### RiftCheckbox.Event:Mouse4Up
Signals that the mouse's fourth button has been released inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Up()
### RiftCheckbox.Event:Mouse4Upoutside
Signals that the mouse's fourth button has been released outside the frame, after a "Down" message. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Upoutside()
### RiftCheckbox.Event:Mouse5Click
Signals that the mouse's fifth button has been clicked inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Click()
### RiftCheckbox.Event:Mouse5Down
Signals that the mouse's fifth button has been pressed inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Down()
### RiftCheckbox.Event:Mouse5Up
Signals that the mouse's fifth button has been released inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Up()
### RiftCheckbox.Event:Mouse5Upoutside
Signals that the mouse's fifth button has been released outside the frame, after a "Down" message. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Upoutside()
### RiftCheckbox.Event:MouseIn
Signals that the mouse cursor has been moved onto the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseIn()
### RiftCheckbox.Event:MouseMove
Signals that the mouse cursor has been moved within the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseMove(x, y)
#### Parameters:
**x** - X coordinate.
**y** - Y coordinate.
### RiftCheckbox.Event:MouseOut
Signals that the mouse cursor has been moved off of the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseOut()
### RiftCheckbox.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### RiftCheckbox.Event:RightClick
Signals that the mouse's right button has been clicked inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightClick()
### RiftCheckbox.Event:RightDown
Signals that the mouse's right button has been pressed inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightDown()
### RiftCheckbox.Event:RightUp
Signals that the mouse's right button has been released inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUp()
### RiftCheckbox.Event:RightUpoutside
Signals that the mouse's right button has been released outside the frame, after a "Down" message. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUpoutside()
### RiftCheckbox.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### RiftCheckbox.Event:Strata
Signals a change in the item's strata.
 
    Frame.Event:Strata()
### RiftCheckbox.Event:WheelBack
Signals that the mousewheel has been moved backward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelBack()
### RiftCheckbox.Event:WheelForward
Signals that the mousewheel has been moved forward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelForward()
### RiftScrollbar.Event:KeyDown
Signals a key pressed.
 
    Frame.Event:KeyDown(button)
#### Parameters:
**button** - The key, in string form.
### RiftScrollbar.Event:KeyFocusGain
Signals gaining key focus.
 
    Frame.Event:KeyFocusGain()
### RiftScrollbar.Event:KeyFocusLoss
Signals losing key focus.
 
    Frame.Event:KeyFocusLoss()
### RiftScrollbar.Event:KeyRepeat
Frame.Event:KeyRepeat@summary
 
    Frame.Event:KeyRepeat(button)
#### Parameters:
**button** - The key, in string form.
### RiftScrollbar.Event:KeyType
Signals text typed.
 
    Frame.Event:KeyType(typed)
#### Parameters:
**typed** - The text.
### RiftScrollbar.Event:KeyUp
Signals a key released.
 
    Frame.Event:KeyUp(button)
#### Parameters:
**button** - The key, in string form.
### RiftScrollbar.Event:Layer
Signals a change in the item's layer.
 
    Frame.Event:Layer()
### RiftScrollbar.Event:LeftClick
Signals that the mouse's left button has been clicked inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftClick()
### RiftScrollbar.Event:LeftDown
Signals that the mouse's left button has been pressed inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftDown()
### RiftScrollbar.Event:LeftUp
Signals that the mouse's left button has been released inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUp()
### RiftScrollbar.Event:LeftUpoutside
Signals that the mouse's left button has been released outside the frame, after a "Down" message. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUpoutside()
### RiftScrollbar.Event:MiddleClick
Signals that the mouse's middle button has been clicked inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleClick()
### RiftScrollbar.Event:MiddleDown
Signals that the mouse's middle button has been pressed inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleDown()
### RiftScrollbar.Event:MiddleUp
Signals that the mouse's middle button has been released inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUp()
### RiftScrollbar.Event:MiddleUpoutside
Signals that the mouse's middle button has been released outside the frame, after a "Down" message. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUpoutside()
### RiftScrollbar.Event:Mouse4Click
Signals that the mouse's fourth button has been clicked inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Click()
### RiftScrollbar.Event:Mouse4Down
Signals that the mouse's fourth button has been pressed inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Down()
### RiftScrollbar.Event:Mouse4Up
Signals that the mouse's fourth button has been released inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Up()
### RiftScrollbar.Event:Mouse4Upoutside
Signals that the mouse's fourth button has been released outside the frame, after a "Down" message. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Upoutside()
### RiftScrollbar.Event:Mouse5Click
Signals that the mouse's fifth button has been clicked inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Click()
### RiftScrollbar.Event:Mouse5Down
Signals that the mouse's fifth button has been pressed inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Down()
### RiftScrollbar.Event:Mouse5Up
Signals that the mouse's fifth button has been released inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Up()
### RiftScrollbar.Event:Mouse5Upoutside
Signals that the mouse's fifth button has been released outside the frame, after a "Down" message. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Upoutside()
### RiftScrollbar.Event:MouseIn
Signals that the mouse cursor has been moved onto the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseIn()
### RiftScrollbar.Event:MouseMove
Signals that the mouse cursor has been moved within the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseMove(x, y)
#### Parameters:
**x** - X coordinate.
**y** - Y coordinate.
### RiftScrollbar.Event:MouseOut
Signals that the mouse cursor has been moved off of the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseOut()
### RiftScrollbar.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### RiftScrollbar.Event:RightClick
Signals that the mouse's right button has been clicked inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightClick()
### RiftScrollbar.Event:RightDown
Signals that the mouse's right button has been pressed inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightDown()
### RiftScrollbar.Event:RightUp
Signals that the mouse's right button has been released inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUp()
### RiftScrollbar.Event:RightUpoutside
Signals that the mouse's right button has been released outside the frame, after a "Down" message. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUpoutside()
### RiftScrollbar.Event:ScrollbarChange
Signals a change in the scrollbar position.
 
    RiftScrollbar.Event:ScrollbarChange()
### RiftScrollbar.Event:ScrollbarGrab
Signals the user grabbing the scrollbar with the mouse.
 
    RiftScrollbar.Event:ScrollbarGrab()
### RiftScrollbar.Event:ScrollbarRelease
Signals the user releasing the scrollbar.
 
    RiftScrollbar.Event:ScrollbarRelease()
### RiftScrollbar.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### RiftScrollbar.Event:Strata
Signals a change in the item's strata.
 
    Frame.Event:Strata()
### RiftScrollbar.Event:WheelBack
Signals that the mousewheel has been moved backward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelBack()
### RiftScrollbar.Event:WheelForward
Signals that the mousewheel has been moved forward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelForward()
### RiftSlider.Event:KeyDown
Signals a key pressed.
 
    Frame.Event:KeyDown(button)
#### Parameters:
**button** - The key, in string form.
### RiftSlider.Event:KeyFocusGain
Signals gaining key focus.
 
    Frame.Event:KeyFocusGain()
### RiftSlider.Event:KeyFocusLoss
Signals losing key focus.
 
    Frame.Event:KeyFocusLoss()
### RiftSlider.Event:KeyRepeat
Frame.Event:KeyRepeat@summary
 
    Frame.Event:KeyRepeat(button)
#### Parameters:
**button** - The key, in string form.
### RiftSlider.Event:KeyType
Signals text typed.
 
    Frame.Event:KeyType(typed)
#### Parameters:
**typed** - The text.
### RiftSlider.Event:KeyUp
Signals a key released.
 
    Frame.Event:KeyUp(button)
#### Parameters:
**button** - The key, in string form.
### RiftSlider.Event:Layer
Signals a change in the item's layer.
 
    Frame.Event:Layer()
### RiftSlider.Event:LeftClick
Signals that the mouse's left button has been clicked inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftClick()
### RiftSlider.Event:LeftDown
Signals that the mouse's left button has been pressed inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftDown()
### RiftSlider.Event:LeftUp
Signals that the mouse's left button has been released inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUp()
### RiftSlider.Event:LeftUpoutside
Signals that the mouse's left button has been released outside the frame, after a "Down" message. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUpoutside()
### RiftSlider.Event:MiddleClick
Signals that the mouse's middle button has been clicked inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleClick()
### RiftSlider.Event:MiddleDown
Signals that the mouse's middle button has been pressed inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleDown()
### RiftSlider.Event:MiddleUp
Signals that the mouse's middle button has been released inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUp()
### RiftSlider.Event:MiddleUpoutside
Signals that the mouse's middle button has been released outside the frame, after a "Down" message. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUpoutside()
### RiftSlider.Event:Mouse4Click
Signals that the mouse's fourth button has been clicked inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Click()
### RiftSlider.Event:Mouse4Down
Signals that the mouse's fourth button has been pressed inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Down()
### RiftSlider.Event:Mouse4Up
Signals that the mouse's fourth button has been released inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Up()
### RiftSlider.Event:Mouse4Upoutside
Signals that the mouse's fourth button has been released outside the frame, after a "Down" message. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Upoutside()
### RiftSlider.Event:Mouse5Click
Signals that the mouse's fifth button has been clicked inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Click()
### RiftSlider.Event:Mouse5Down
Signals that the mouse's fifth button has been pressed inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Down()
### RiftSlider.Event:Mouse5Up
Signals that the mouse's fifth button has been released inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Up()
### RiftSlider.Event:Mouse5Upoutside
Signals that the mouse's fifth button has been released outside the frame, after a "Down" message. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Upoutside()
### RiftSlider.Event:MouseIn
Signals that the mouse cursor has been moved onto the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseIn()
### RiftSlider.Event:MouseMove
Signals that the mouse cursor has been moved within the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseMove(x, y)
#### Parameters:
**x** - X coordinate.
**y** - Y coordinate.
### RiftSlider.Event:MouseOut
Signals that the mouse cursor has been moved off of the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseOut()
### RiftSlider.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### RiftSlider.Event:RightClick
Signals that the mouse's right button has been clicked inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightClick()
### RiftSlider.Event:RightDown
Signals that the mouse's right button has been pressed inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightDown()
### RiftSlider.Event:RightUp
Signals that the mouse's right button has been released inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUp()
### RiftSlider.Event:RightUpoutside
Signals that the mouse's right button has been released outside the frame, after a "Down" message. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUpoutside()
### RiftSlider.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### RiftSlider.Event:SliderChange
Signals a change in the slider position.
 
    RiftSlider.Event:SliderChange()
### RiftSlider.Event:SliderGrab
Signals the user grabbing the slider with the mouse.
 
    RiftSlider.Event:SliderGrab()
### RiftSlider.Event:SliderRelease
Signals the user releasing the slider.
 
    RiftSlider.Event:SliderRelease()
### RiftSlider.Event:Strata
Signals a change in the item's strata.
 
    Frame.Event:Strata()
### RiftSlider.Event:WheelBack
Signals that the mousewheel has been moved backward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelBack()
### RiftSlider.Event:WheelForward
Signals that the mousewheel has been moved forward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelForward()
### RiftTextfield.Event:KeyDown
Signals a key pressed.
 
    Frame.Event:KeyDown(button)
#### Parameters:
**button** - The key, in string form.
### RiftTextfield.Event:KeyFocusGain
Signals gaining key focus.
 
    Frame.Event:KeyFocusGain()
### RiftTextfield.Event:KeyFocusLoss
Signals losing key focus.
 
    Frame.Event:KeyFocusLoss()
### RiftTextfield.Event:KeyRepeat
Frame.Event:KeyRepeat@summary
 
    Frame.Event:KeyRepeat(button)
#### Parameters:
**button** - The key, in string form.
### RiftTextfield.Event:KeyType
Signals text typed.
 
    Frame.Event:KeyType(typed)
#### Parameters:
**typed** - The text.
### RiftTextfield.Event:KeyUp
Signals a key released.
 
    Frame.Event:KeyUp(button)
#### Parameters:
**button** - The key, in string form.
### RiftTextfield.Event:Layer
Signals a change in the item's layer.
 
    Frame.Event:Layer()
### RiftTextfield.Event:LeftClick
Signals that the mouse's left button has been clicked inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftClick()
### RiftTextfield.Event:LeftDown
Signals that the mouse's left button has been pressed inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftDown()
### RiftTextfield.Event:LeftUp
Signals that the mouse's left button has been released inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUp()
### RiftTextfield.Event:LeftUpoutside
Signals that the mouse's left button has been released outside the frame, after a "Down" message. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUpoutside()
### RiftTextfield.Event:MiddleClick
Signals that the mouse's middle button has been clicked inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleClick()
### RiftTextfield.Event:MiddleDown
Signals that the mouse's middle button has been pressed inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleDown()
### RiftTextfield.Event:MiddleUp
Signals that the mouse's middle button has been released inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUp()
### RiftTextfield.Event:MiddleUpoutside
Signals that the mouse's middle button has been released outside the frame, after a "Down" message. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUpoutside()
### RiftTextfield.Event:Mouse4Click
Signals that the mouse's fourth button has been clicked inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Click()
### RiftTextfield.Event:Mouse4Down
Signals that the mouse's fourth button has been pressed inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Down()
### RiftTextfield.Event:Mouse4Up
Signals that the mouse's fourth button has been released inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Up()
### RiftTextfield.Event:Mouse4Upoutside
Signals that the mouse's fourth button has been released outside the frame, after a "Down" message. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Upoutside()
### RiftTextfield.Event:Mouse5Click
Signals that the mouse's fifth button has been clicked inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Click()
### RiftTextfield.Event:Mouse5Down
Signals that the mouse's fifth button has been pressed inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Down()
### RiftTextfield.Event:Mouse5Up
Signals that the mouse's fifth button has been released inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Up()
### RiftTextfield.Event:Mouse5Upoutside
Signals that the mouse's fifth button has been released outside the frame, after a "Down" message. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Upoutside()
### RiftTextfield.Event:MouseIn
Signals that the mouse cursor has been moved onto the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseIn()
### RiftTextfield.Event:MouseMove
Signals that the mouse cursor has been moved within the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseMove(x, y)
#### Parameters:
**x** - X coordinate.
**y** - Y coordinate.
### RiftTextfield.Event:MouseOut
Signals that the mouse cursor has been moved off of the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseOut()
### RiftTextfield.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### RiftTextfield.Event:RightClick
Signals that the mouse's right button has been clicked inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightClick()
### RiftTextfield.Event:RightDown
Signals that the mouse's right button has been pressed inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightDown()
### RiftTextfield.Event:RightUp
Signals that the mouse's right button has been released inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUp()
### RiftTextfield.Event:RightUpoutside
Signals that the mouse's right button has been released outside the frame, after a "Down" message. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUpoutside()
### RiftTextfield.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### RiftTextfield.Event:Strata
Signals a change in the item's strata.
 
    Frame.Event:Strata()
### RiftTextfield.Event:TextfieldChange
Signals that the textfield's text has changed.
 
    RiftTextfield.Event:TextfieldChange()
### RiftTextfield.Event:TextfieldSelect
Signals that the textfield's selection has changed.
 
    RiftTextfield.Event:TextfieldSelect()
### RiftTextfield.Event:WheelBack
Signals that the mousewheel has been moved backward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelBack()
### RiftTextfield.Event:WheelForward
Signals that the mousewheel has been moved forward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelForward()
### RiftWindow.Event:KeyDown
Signals a key pressed.
 
    Frame.Event:KeyDown(button)
#### Parameters:
**button** - The key, in string form.
### RiftWindow.Event:KeyFocusGain
Signals gaining key focus.
 
    Frame.Event:KeyFocusGain()
### RiftWindow.Event:KeyFocusLoss
Signals losing key focus.
 
    Frame.Event:KeyFocusLoss()
### RiftWindow.Event:KeyRepeat
Frame.Event:KeyRepeat@summary
 
    Frame.Event:KeyRepeat(button)
#### Parameters:
**button** - The key, in string form.
### RiftWindow.Event:KeyType
Signals text typed.
 
    Frame.Event:KeyType(typed)
#### Parameters:
**typed** - The text.
### RiftWindow.Event:KeyUp
Signals a key released.
 
    Frame.Event:KeyUp(button)
#### Parameters:
**button** - The key, in string form.
### RiftWindow.Event:Layer
Signals a change in the item's layer.
 
    Frame.Event:Layer()
### RiftWindow.Event:LeftClick
Signals that the mouse's left button has been clicked inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftClick()
### RiftWindow.Event:LeftDown
Signals that the mouse's left button has been pressed inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftDown()
### RiftWindow.Event:LeftUp
Signals that the mouse's left button has been released inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUp()
### RiftWindow.Event:LeftUpoutside
Signals that the mouse's left button has been released outside the frame, after a "Down" message. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUpoutside()
### RiftWindow.Event:MiddleClick
Signals that the mouse's middle button has been clicked inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleClick()
### RiftWindow.Event:MiddleDown
Signals that the mouse's middle button has been pressed inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleDown()
### RiftWindow.Event:MiddleUp
Signals that the mouse's middle button has been released inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUp()
### RiftWindow.Event:MiddleUpoutside
Signals that the mouse's middle button has been released outside the frame, after a "Down" message. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUpoutside()
### RiftWindow.Event:Mouse4Click
Signals that the mouse's fourth button has been clicked inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Click()
### RiftWindow.Event:Mouse4Down
Signals that the mouse's fourth button has been pressed inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Down()
### RiftWindow.Event:Mouse4Up
Signals that the mouse's fourth button has been released inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Up()
### RiftWindow.Event:Mouse4Upoutside
Signals that the mouse's fourth button has been released outside the frame, after a "Down" message. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Upoutside()
### RiftWindow.Event:Mouse5Click
Signals that the mouse's fifth button has been clicked inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Click()
### RiftWindow.Event:Mouse5Down
Signals that the mouse's fifth button has been pressed inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Down()
### RiftWindow.Event:Mouse5Up
Signals that the mouse's fifth button has been released inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Up()
### RiftWindow.Event:Mouse5Upoutside
Signals that the mouse's fifth button has been released outside the frame, after a "Down" message. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Upoutside()
### RiftWindow.Event:MouseIn
Signals that the mouse cursor has been moved onto the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseIn()
### RiftWindow.Event:MouseMove
Signals that the mouse cursor has been moved within the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseMove(x, y)
#### Parameters:
**x** - X coordinate.
**y** - Y coordinate.
### RiftWindow.Event:MouseOut
Signals that the mouse cursor has been moved off of the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseOut()
### RiftWindow.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### RiftWindow.Event:RightClick
Signals that the mouse's right button has been clicked inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightClick()
### RiftWindow.Event:RightDown
Signals that the mouse's right button has been pressed inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightDown()
### RiftWindow.Event:RightUp
Signals that the mouse's right button has been released inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUp()
### RiftWindow.Event:RightUpoutside
Signals that the mouse's right button has been released outside the frame, after a "Down" message. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUpoutside()
### RiftWindow.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### RiftWindow.Event:Strata
Signals a change in the item's strata.
 
    Frame.Event:Strata()
### RiftWindow.Event:WheelBack
Signals that the mousewheel has been moved backward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelBack()
### RiftWindow.Event:WheelForward
Signals that the mousewheel has been moved forward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelForward()
### RiftWindowBorder.Event:KeyDown
Signals a key pressed.
 
    Frame.Event:KeyDown(button)
#### Parameters:
**button** - The key, in string form.
### RiftWindowBorder.Event:KeyFocusGain
Signals gaining key focus.
 
    Frame.Event:KeyFocusGain()
### RiftWindowBorder.Event:KeyFocusLoss
Signals losing key focus.
 
    Frame.Event:KeyFocusLoss()
### RiftWindowBorder.Event:KeyRepeat
Frame.Event:KeyRepeat@summary
 
    Frame.Event:KeyRepeat(button)
#### Parameters:
**button** - The key, in string form.
### RiftWindowBorder.Event:KeyType
Signals text typed.
 
    Frame.Event:KeyType(typed)
#### Parameters:
**typed** - The text.
### RiftWindowBorder.Event:KeyUp
Signals a key released.
 
    Frame.Event:KeyUp(button)
#### Parameters:
**button** - The key, in string form.
### RiftWindowBorder.Event:LeftClick
Signals that the mouse's left button has been clicked inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftClick()
### RiftWindowBorder.Event:LeftDown
Signals that the mouse's left button has been pressed inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftDown()
### RiftWindowBorder.Event:LeftUp
Signals that the mouse's left button has been released inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUp()
### RiftWindowBorder.Event:LeftUpoutside
Signals that the mouse's left button has been released outside the frame, after a "Down" message. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUpoutside()
### RiftWindowBorder.Event:MiddleClick
Signals that the mouse's middle button has been clicked inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleClick()
### RiftWindowBorder.Event:MiddleDown
Signals that the mouse's middle button has been pressed inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleDown()
### RiftWindowBorder.Event:MiddleUp
Signals that the mouse's middle button has been released inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUp()
### RiftWindowBorder.Event:MiddleUpoutside
Signals that the mouse's middle button has been released outside the frame, after a "Down" message. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUpoutside()
### RiftWindowBorder.Event:Mouse4Click
Signals that the mouse's fourth button has been clicked inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Click()
### RiftWindowBorder.Event:Mouse4Down
Signals that the mouse's fourth button has been pressed inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Down()
### RiftWindowBorder.Event:Mouse4Up
Signals that the mouse's fourth button has been released inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Up()
### RiftWindowBorder.Event:Mouse4Upoutside
Signals that the mouse's fourth button has been released outside the frame, after a "Down" message. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Upoutside()
### RiftWindowBorder.Event:Mouse5Click
Signals that the mouse's fifth button has been clicked inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Click()
### RiftWindowBorder.Event:Mouse5Down
Signals that the mouse's fifth button has been pressed inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Down()
### RiftWindowBorder.Event:Mouse5Up
Signals that the mouse's fifth button has been released inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Up()
### RiftWindowBorder.Event:Mouse5Upoutside
Signals that the mouse's fifth button has been released outside the frame, after a "Down" message. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Upoutside()
### RiftWindowBorder.Event:MouseIn
Signals that the mouse cursor has been moved onto the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseIn()
### RiftWindowBorder.Event:MouseMove
Signals that the mouse cursor has been moved within the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseMove(x, y)
#### Parameters:
**x** - X coordinate.
**y** - Y coordinate.
### RiftWindowBorder.Event:MouseOut
Signals that the mouse cursor has been moved off of the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseOut()
### RiftWindowBorder.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### RiftWindowBorder.Event:RightClick
Signals that the mouse's right button has been clicked inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightClick()
### RiftWindowBorder.Event:RightDown
Signals that the mouse's right button has been pressed inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightDown()
### RiftWindowBorder.Event:RightUp
Signals that the mouse's right button has been released inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUp()
### RiftWindowBorder.Event:RightUpoutside
Signals that the mouse's right button has been released outside the frame, after a "Down" message. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUpoutside()
### RiftWindowBorder.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### RiftWindowBorder.Event:WheelBack
Signals that the mousewheel has been moved backward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelBack()
### RiftWindowBorder.Event:WheelForward
Signals that the mousewheel has been moved forward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelForward()
### RiftWindowContent.Event:KeyDown
Signals a key pressed.
 
    Frame.Event:KeyDown(button)
#### Parameters:
**button** - The key, in string form.
### RiftWindowContent.Event:KeyFocusGain
Signals gaining key focus.
 
    Frame.Event:KeyFocusGain()
### RiftWindowContent.Event:KeyFocusLoss
Signals losing key focus.
 
    Frame.Event:KeyFocusLoss()
### RiftWindowContent.Event:KeyRepeat
Frame.Event:KeyRepeat@summary
 
    Frame.Event:KeyRepeat(button)
#### Parameters:
**button** - The key, in string form.
### RiftWindowContent.Event:KeyType
Signals text typed.
 
    Frame.Event:KeyType(typed)
#### Parameters:
**typed** - The text.
### RiftWindowContent.Event:KeyUp
Signals a key released.
 
    Frame.Event:KeyUp(button)
#### Parameters:
**button** - The key, in string form.
### RiftWindowContent.Event:LeftClick
Signals that the mouse's left button has been clicked inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftClick()
### RiftWindowContent.Event:LeftDown
Signals that the mouse's left button has been pressed inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftDown()
### RiftWindowContent.Event:LeftUp
Signals that the mouse's left button has been released inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUp()
### RiftWindowContent.Event:LeftUpoutside
Signals that the mouse's left button has been released outside the frame, after a "Down" message. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUpoutside()
### RiftWindowContent.Event:MiddleClick
Signals that the mouse's middle button has been clicked inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleClick()
### RiftWindowContent.Event:MiddleDown
Signals that the mouse's middle button has been pressed inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleDown()
### RiftWindowContent.Event:MiddleUp
Signals that the mouse's middle button has been released inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUp()
### RiftWindowContent.Event:MiddleUpoutside
Signals that the mouse's middle button has been released outside the frame, after a "Down" message. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUpoutside()
### RiftWindowContent.Event:Mouse4Click
Signals that the mouse's fourth button has been clicked inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Click()
### RiftWindowContent.Event:Mouse4Down
Signals that the mouse's fourth button has been pressed inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Down()
### RiftWindowContent.Event:Mouse4Up
Signals that the mouse's fourth button has been released inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Up()
### RiftWindowContent.Event:Mouse4Upoutside
Signals that the mouse's fourth button has been released outside the frame, after a "Down" message. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Upoutside()
### RiftWindowContent.Event:Mouse5Click
Signals that the mouse's fifth button has been clicked inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Click()
### RiftWindowContent.Event:Mouse5Down
Signals that the mouse's fifth button has been pressed inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Down()
### RiftWindowContent.Event:Mouse5Up
Signals that the mouse's fifth button has been released inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Up()
### RiftWindowContent.Event:Mouse5Upoutside
Signals that the mouse's fifth button has been released outside the frame, after a "Down" message. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Upoutside()
### RiftWindowContent.Event:MouseIn
Signals that the mouse cursor has been moved onto the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseIn()
### RiftWindowContent.Event:MouseMove
Signals that the mouse cursor has been moved within the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseMove(x, y)
#### Parameters:
**x** - X coordinate.
**y** - Y coordinate.
### RiftWindowContent.Event:MouseOut
Signals that the mouse cursor has been moved off of the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseOut()
### RiftWindowContent.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### RiftWindowContent.Event:RightClick
Signals that the mouse's right button has been clicked inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightClick()
### RiftWindowContent.Event:RightDown
Signals that the mouse's right button has been pressed inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightDown()
### RiftWindowContent.Event:RightUp
Signals that the mouse's right button has been released inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUp()
### RiftWindowContent.Event:RightUpoutside
Signals that the mouse's right button has been released outside the frame, after a "Down" message. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUpoutside()
### RiftWindowContent.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### RiftWindowContent.Event:WheelBack
Signals that the mousewheel has been moved backward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelBack()
### RiftWindowContent.Event:WheelForward
Signals that the mousewheel has been moved forward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelForward()
### Text.Event:KeyDown
Signals a key pressed.
 
    Frame.Event:KeyDown(button)
#### Parameters:
**button** - The key, in string form.
### Text.Event:KeyFocusGain
Signals gaining key focus.
 
    Frame.Event:KeyFocusGain()
### Text.Event:KeyFocusLoss
Signals losing key focus.
 
    Frame.Event:KeyFocusLoss()
### Text.Event:KeyRepeat
Frame.Event:KeyRepeat@summary
 
    Frame.Event:KeyRepeat(button)
#### Parameters:
**button** - The key, in string form.
### Text.Event:KeyType
Signals text typed.
 
    Frame.Event:KeyType(typed)
#### Parameters:
**typed** - The text.
### Text.Event:KeyUp
Signals a key released.
 
    Frame.Event:KeyUp(button)
#### Parameters:
**button** - The key, in string form.
### Text.Event:Layer
Signals a change in the item's layer.
 
    Frame.Event:Layer()
### Text.Event:LeftClick
Signals that the mouse's left button has been clicked inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftClick()
### Text.Event:LeftDown
Signals that the mouse's left button has been pressed inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftDown()
### Text.Event:LeftUp
Signals that the mouse's left button has been released inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUp()
### Text.Event:LeftUpoutside
Signals that the mouse's left button has been released outside the frame, after a "Down" message. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUpoutside()
### Text.Event:MiddleClick
Signals that the mouse's middle button has been clicked inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleClick()
### Text.Event:MiddleDown
Signals that the mouse's middle button has been pressed inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleDown()
### Text.Event:MiddleUp
Signals that the mouse's middle button has been released inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUp()
### Text.Event:MiddleUpoutside
Signals that the mouse's middle button has been released outside the frame, after a "Down" message. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUpoutside()
### Text.Event:Mouse4Click
Signals that the mouse's fourth button has been clicked inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Click()
### Text.Event:Mouse4Down
Signals that the mouse's fourth button has been pressed inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Down()
### Text.Event:Mouse4Up
Signals that the mouse's fourth button has been released inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Up()
### Text.Event:Mouse4Upoutside
Signals that the mouse's fourth button has been released outside the frame, after a "Down" message. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Upoutside()
### Text.Event:Mouse5Click
Signals that the mouse's fifth button has been clicked inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Click()
### Text.Event:Mouse5Down
Signals that the mouse's fifth button has been pressed inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Down()
### Text.Event:Mouse5Up
Signals that the mouse's fifth button has been released inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Up()
### Text.Event:Mouse5Upoutside
Signals that the mouse's fifth button has been released outside the frame, after a "Down" message. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Upoutside()
### Text.Event:MouseIn
Signals that the mouse cursor has been moved onto the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseIn()
### Text.Event:MouseMove
Signals that the mouse cursor has been moved within the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseMove(x, y)
#### Parameters:
**x** - X coordinate.
**y** - Y coordinate.
### Text.Event:MouseOut
Signals that the mouse cursor has been moved off of the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseOut()
### Text.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### Text.Event:RightClick
Signals that the mouse's right button has been clicked inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightClick()
### Text.Event:RightDown
Signals that the mouse's right button has been pressed inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightDown()
### Text.Event:RightUp
Signals that the mouse's right button has been released inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUp()
### Text.Event:RightUpoutside
Signals that the mouse's right button has been released outside the frame, after a "Down" message. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUpoutside()
### Text.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### Text.Event:Strata
Signals a change in the item's strata.
 
    Frame.Event:Strata()
### Text.Event:WheelBack
Signals that the mousewheel has been moved backward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelBack()
### Text.Event:WheelForward
Signals that the mousewheel has been moved forward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelForward()
### Texture.Event:KeyDown
Signals a key pressed.
 
    Frame.Event:KeyDown(button)
#### Parameters:
**button** - The key, in string form.
### Texture.Event:KeyFocusGain
Signals gaining key focus.
 
    Frame.Event:KeyFocusGain()
### Texture.Event:KeyFocusLoss
Signals losing key focus.
 
    Frame.Event:KeyFocusLoss()
### Texture.Event:KeyRepeat
Frame.Event:KeyRepeat@summary
 
    Frame.Event:KeyRepeat(button)
#### Parameters:
**button** - The key, in string form.
### Texture.Event:KeyType
Signals text typed.
 
    Frame.Event:KeyType(typed)
#### Parameters:
**typed** - The text.
### Texture.Event:KeyUp
Signals a key released.
 
    Frame.Event:KeyUp(button)
#### Parameters:
**button** - The key, in string form.
### Texture.Event:Layer
Signals a change in the item's layer.
 
    Frame.Event:Layer()
### Texture.Event:LeftClick
Signals that the mouse's left button has been clicked inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftClick()
### Texture.Event:LeftDown
Signals that the mouse's left button has been pressed inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftDown()
### Texture.Event:LeftUp
Signals that the mouse's left button has been released inside the frame. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUp()
### Texture.Event:LeftUpoutside
Signals that the mouse's left button has been released outside the frame, after a "Down" message. Blocks left mouse events and mouse movement events from frames below this one. May block right mouse events depending on the MouseMasking mode.
 
    Frame.Event:LeftUpoutside()
### Texture.Event:MiddleClick
Signals that the mouse's middle button has been clicked inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleClick()
### Texture.Event:MiddleDown
Signals that the mouse's middle button has been pressed inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleDown()
### Texture.Event:MiddleUp
Signals that the mouse's middle button has been released inside the frame. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUp()
### Texture.Event:MiddleUpoutside
Signals that the mouse's middle button has been released outside the frame, after a "Down" message. Blocks middle mouse events from frames below this one.
 
    Frame.Event:MiddleUpoutside()
### Texture.Event:Mouse4Click
Signals that the mouse's fourth button has been clicked inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Click()
### Texture.Event:Mouse4Down
Signals that the mouse's fourth button has been pressed inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Down()
### Texture.Event:Mouse4Up
Signals that the mouse's fourth button has been released inside the frame. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Up()
### Texture.Event:Mouse4Upoutside
Signals that the mouse's fourth button has been released outside the frame, after a "Down" message. Blocks fourth button mouse events from frames below this one.
 
    Frame.Event:Mouse4Upoutside()
### Texture.Event:Mouse5Click
Signals that the mouse's fifth button has been clicked inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Click()
### Texture.Event:Mouse5Down
Signals that the mouse's fifth button has been pressed inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Down()
### Texture.Event:Mouse5Up
Signals that the mouse's fifth button has been released inside the frame. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Up()
### Texture.Event:Mouse5Upoutside
Signals that the mouse's fifth button has been released outside the frame, after a "Down" message. Blocks fifth button mouse events from frames below this one.
 
    Frame.Event:Mouse5Upoutside()
### Texture.Event:MouseIn
Signals that the mouse cursor has been moved onto the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseIn()
### Texture.Event:MouseMove
Signals that the mouse cursor has been moved within the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseMove(x, y)
#### Parameters:
**x** - X coordinate.
**y** - Y coordinate.
### Texture.Event:MouseOut
Signals that the mouse cursor has been moved off of the frame. Blocks mouse movement events from frames below this one. May block left and right events depending on the MouseMasking mode.
 
    Frame.Event:MouseOut()
### Texture.Event:Move
Signals that the frame's vertices have moved.
 
    Layout.Event:Move()
### Texture.Event:RightClick
Signals that the mouse's right button has been clicked inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightClick()
### Texture.Event:RightDown
Signals that the mouse's right button has been pressed inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightDown()
### Texture.Event:RightUp
Signals that the mouse's right button has been released inside the frame. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUp()
### Texture.Event:RightUpoutside
Signals that the mouse's right button has been released outside the frame, after a "Down" message. Blocks right mouse events and mouse movement events from frames below this one. May block left mouse events depending on the MouseMasking mode.
 
    Frame.Event:RightUpoutside()
### Texture.Event:Size
Signals that the frame's size has changed.
 
    Layout.Event:Size()
### Texture.Event:Strata
Signals a change in the item's strata.
 
    Frame.Event:Strata()
### Texture.Event:WheelBack
Signals that the mousewheel has been moved backward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelBack()
### Texture.Event:WheelForward
Signals that the mousewheel has been moved forward inside the frame. Blocks mousewheel events from frames below this one.
 
    Frame.Event:WheelForward()
## dump
## Types
### ability
An ability, represented by a standard identifier with an 'a' prefix.
### achievement
An achievement, represented by a standard identifier with a 'c' prefix.
### achievementcategory
An achievement category, represented by a standard identifier with a 'C' prefix.
### auction
An auction, represented by a standard identifier with an 'o' prefix.
### boolean
A value of type 'boolean'.
### buff
A buff, represented by a standard identifier with a 'b' prefix.
### callbackfunction
A callback function, used to signal success or failure. Must be a function that takes two parameters, "failure" and "message". On success, "failure" will be false or nil. Otherwise, "failure" will be either a computer-readable string identifier or true. "message" may contain a human-readable message that may be delivered to the user.
### Canvas
A UI element of type Canvas or derived from Canvas.
### console
A text console, represented by a standard ID with a 'v' prefix.
### Context
A UI element of type Context or derived from Context.
### currencycategory
A currency category, represented by a standard identifier with a 'N' prefix.
### dimensionitem
A dimension item, represented by a standard identifier with a 'd' prefix.
### Element
A UI element of type Element or derived from Element.
### error
An error, represented by a standard ID with an 'x' prefix.
### eventFrame
A frame event handle.
### eventGlobal
A global event handle.
### faction
A faction, represented by a standard ID with an 'f' prefix.
### Frame
A UI element of type Frame or derived from Frame.
### frameEventTable
A frame event table of a UI element.
### function
A value of type 'function'.
### function/nil
A value of type "function" or "nil".
### function/string
A value of type "function" or "string".
### function/string/nil
A value of type "function", "string", or "nil".
### guildrank
A guild rank, represented by a standard identifier with a 'k' prefix.
### guildwall
A guild wall ID, represented by a standard identifier with a 'w' prefix.
### handleEventFrame
A handle for active frame events. Includes the member :GetTarget(), which returns the target frame of this event. If being passed to a .Dive handler, also includes :Halt() and :Catch().  :Halt() halts propogation of the event through the hierarchy and, after finishing the current frame's .Dive handlers, immediately begins processing .Bubble events. :Catch() works similarly but processes main events before continuing to .Bubble. Neither function may be called when the environment is in secure mode and either the current or target frame has a restricted secure mode.
### handleEventGlobal
A handle for active global events. Currently contains no members.
### item
An item, represented by either a standard identifier with an 'i' prefix or a slot.
### itemtype
An item type, represented by a nonstandard identifier with an 'I' prefix.
### Layout
A UI element of type Layout or derived from Layout.
### location
A map location, represented by a nonstandard identifier with an 'l' prefix.
### mail
A mail, represented by a standard identifier with an 'm' prefix.
### Mask
A UI element of type Mask or derived from Mask.
### minionadventure
A minion adventure, represented by a standard identifier with a 'mn.adv.' prefix.
### minionminion
A minion, represented by a standard identifier with a 'mn.mn.' prefix.
### Native
A UI element of type Native or derived from Native.
### nil
A value of type 'nil'.
### number
A value of type 'number'. May not be NaN.
### number/nil
A value of type 'number' or 'nil'. May not be NaN.
### quest
A quest, represented by a standard identifier with a 'q' prefix.
### RiftButton
A UI element of type RiftButton or derived from RiftButton.
### RiftCheckbox
A UI element of type RiftCheckbox or derived from RiftCheckbox.
### RiftScrollbar
A UI element of type RiftScrollbar or derived from RiftScrollbar.
### RiftSlider
A UI element of type RiftSlider or derived from RiftSlider.
### RiftTextfield
A UI element of type RiftTextfield or derived from RiftTextfield.
### RiftWindow
A UI element of type RiftWindow or derived from RiftWindow.
### role
A role, represented by a standard identifier with a "h" prefix.
### slot
An item slot, represented by a nonstandard identifier with an 's' prefix, as generated by the Utility.Item.Slot.* functions.
### string
A value of type 'string'.
### string/nil
A value of type 'string' or 'nil'.
### table
A value of type 'table'.
### Text
A UI element of type Text or derived from Text.
### Texture
A UI element of type Texture or derived from Texture.
### title
A title, represented by a standard identifier with a 't' prefix.
### titlecategory
A title category, represented by a standard identifier with a 'T' prefix.
### unit
A unit, represented by either a standard identifier with a 'u' prefix or by a unit specifier. Unit specifiers start with any of "player", "focus", "mouseover", "group01" through "group20", or a standard identifier with a 'u' prefix, and are followed by a chain of any number of ".target", ".pet", or ".owner" modifiers.
### variant
A value of any type.
### zone
A zone, represented by a standard identifier with a 'z' prefix.
(eof)
