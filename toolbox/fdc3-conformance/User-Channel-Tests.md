# User Channel Tests  ![1.2](https://img.shields.io/badge/FDC3-1.2-green) ![2.0](https://img.shields.io/badge/FDC3-2.0-blue)

_NB:  User Channels were called System Channels in FDC3 1.2.  The new terminology is used in this specification_


## Basic Broadcast

| App | Step               |Details                                                                           |
|-----|--------------------|----------------------------------------------------------------------------------|
| A   | 1.addContextListener |A adds an _untyped_ Context Listene using `addContextListener(null, handler)`. <br/>![1.2](https://img.shields.io/badge/FDC3-1.2-green) A `Listener` object is returned  <br />![2.0](https://img.shields.io/badge/FDC3-2.0-blue) A promise resolving a `Listener` object is returned <br />Check that this has an `unsubscribe` method. |
| A   | 2.joinUserChannel     |A joins the first available user channel using: <br/>![1.2](https://img.shields.io/badge/FDC3-1.2-green) `getSystemChannels()` Check channels are returned. <br/>![2.0](https://img.shields.io/badge/FDC3-2.0-blue) `getUserChannels()` Check **user** channels are returned.<br/>Call `fdc3.joinChannel()` on the first non-global channel. |
| B   | 3.joinUserChannel     |B joins the same channel as A, via the same process in 2. |
| B   | 4.Broadcast          | B broadcasts an `fdc3.instrument` context to the channel using `fdc3.broadcast(<the instrument>)`. <br/>![2.0](https://img.shields.io/badge/FDC3-2.0-blue)  Check a `void` promise is returned. |
| A   | 5.Receive Context    | A receives the instrument object, matching the one broadcast by B.  |

- `UCBasicUsage1` Perform above test 
- `UCBasicUsage2` Perform steps in order: 2,1,3,4,5
- `UCBasicUsage3` Perform steps in order: 3,4,1,2,5
- `UCBasicUsage4` Perform steps in order: 3,4,2,1,5

## Filtered Broadcast

| App | Step               |Details                                                                           |
|-----|--------------------|----------------------------------------------------------------------------------|
| A   | 1.addContextListener |A adds a `fdc3.instrument` _typed_ Context Listener using `addContextListener("fdc3.instrument", handler)`. <br/>![1.2](https://img.shields.io/badge/FDC3-1.2-green) A `Listener` object is returned  <br />![2.0](https://img.shields.io/badge/FDC3-2.0-blue) A promise resolving a `Listener` object is returned <br />Check that this has an `unsubscribe` method.|
| A   | 2.joinUserChannel     |A joins the first available user channel using: <br/> ![1.2](https://img.shields.io/badge/FDC3-1.2-green) `getSystemChannels()` Check channels are returned. <br/>![2.0](https://img.shields.io/badge/FDC3-2.0-blue) `getUserChannels()` Check **user** channels are returned.<br/>Call `fdc3.joinChannel()` on the first non-global channel.|
| B   | 3.joinUserChannel     |B joins the same channel as A, via the same process in 2. |
| B   | 4.Broadcast          | B broadcasts: <br/> 1.`fdc3.broadcast(<the instrument>)`. <br/> 2. `fdc3.broadcast(<a contact>)` <br /> ![2.0](https://img.shields.io/badge/FDC3-2.0-blue)  Check a `void` promise is returned. |
| A   | 5.Receive Context    | A receives the `fdc3.instrument` object, matching the one broadcast by B. <br />Check that the `fdc3.contact` is not received. |

- `UCFilteredUsage1` Perform above test 
- `UCFilteredUsage2` Perform steps in order: 2,1,3,4,5
- `UCFilteredUsage3` Perform steps in order: 3,4,1,2,5
- `UCFilteredUsage4` Perform steps in order: 3,4,2,1,5

## Broadcast With Multiple Listeners

| App | Step               | Details                                                                                                     |
|-----|--------------------|-------------------------------------------------------------------------------------------------------------|
| A   | 1.addContextListeners | A sets up two Context Listeners.  One for `fdc3.instrument` and one for `fdc3.contact` by calling:  `addContextListener ("fdc3.instrument", handler)` <br/> `addContextListener ("fdc3.contact", handler)` <br/>![1.2](https://img.shields.io/badge/FDC3-1.2-green) A `Listener` object is returned for each.  <br />![2.0](https://img.shields.io/badge/FDC3-2.0-blue) A promise resolving a `Listener` object is returned for each. <br />Check that this has an `unsubscribe` method for each.  |
| A   | 2.joinUserChannel     |A joins the first available user channel using: <br/>![1.2](https://img.shields.io/badge/FDC3-1.2-green) `getSystemChannels()` Check channels are returned. <br/>![2.0](https://img.shields.io/badge/FDC3-2.0-blue) `getUserChannels()` Check **user** channels are returned.<br/>Call `fdc3.joinChannel()` on the first non-global channel.|
| B   | 3.joinUserChannel     |B joins the same channel as A, via the same process in 2. |
| B   | 4.Broadcast          | `fdc.broadcast(<instrument context>)` <br/> `fdc3.broadcast(<contact context>)` . |
| A   | 5.Receive Context    | A's `fdc3.instrument` object matches the one broadcast by B, and arrives on the correct listener.<br>A's `fdc3.context` object matches the one broadcast  by B, and arrives on the correct listener.   |

 - `UCFilteredUsage5`: Perform above test
 - `UCFilteredUsage6`: Perform above test, except B will join a _different_ channel to A. Check that you _don't_ receive anything.
 - `UCFilteredUsageChange`: Perform above test, except that after joining, **A** changes channel to a _different_ channel via a further call to `fdc3.joinUserChannel`.  Check that **A** _doesn't_ receive anything.
 - `UCFilteredUsageUnsubscribe`: Perform above test, except that after joining, **A** then `unsubscribe()`s from the channel using the `listener.unsubscribe` function. Check that **A** _doesn't_ receive anything. 
 - `UCFilteredUsageLeave`: Perform above test, except that immediately after joining, **A** _leaves the channel_, and so receives nothing.
 - `UCFilteredUsageNoJoin`: Perform the above test, but skip step 2 so that **A** doesn't join a channel. Confirm that the _current channel_ for **A** is not set before continuing with the rest of the test.  **A** should receive nothing.