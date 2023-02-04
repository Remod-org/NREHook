# NREHook

Using Harmony, we patch uMod's CallHook function to signal a plugin that an NullReferenceException has occurred in the plugin.

This looks for an exception, which is normally logged to the server logs, and adds yet another CallHook:

```cs
void Interface.Oxide.CallHook("OnFoundNRE", string pluginName)
```

At this point (Feb 2023), we are only able to signal the plugin that caused the NRE, but this might be adjustable to call out to other plugins for further diagnostics.

This patch is very small, and operates as an unloadable patch in a plugin format.

## USAGE

This is only useful for devs who may wish to integrate automated logging, etc. for their plugins in cases where NRE's are found.

In your plugin, add the following:

```cs
	private void OnFoundNRE(string plugin)
	{
		// Do something in response to the NRE, e.g.:
		// debug = true;
	}
```

## Design Goal

The purpose of this plugin was to help diagnose a long-standing issue with my plugin, NextGenPVE.  With this patch, I can now (optionally) enable debugging at the point that the NRE occurs.

I am adding a function to accept feedback from this new hook so that I can temporarily enable debugging for 30 seconds.

