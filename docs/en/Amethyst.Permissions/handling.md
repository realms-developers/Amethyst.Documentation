# Permission Handling

## What is PermissionAccess?

`PermissionAccess` is an `enum` used to handle permission check results.

```cs
public enum PermissionAccess
{
    // Skips the check
    // In modules like Groups, this is returned when the player's group lacks permission.
    None,

    // Indicates the object (e.g., NetPlayer) has the permission
    HasPermission,

    // Indicates the object (e.g., NetPlayer) must critically NOT have the permission
    // Use this sparingly, as overuse may break other plugins/modules
    Blocked
}
```

To extend permissions for `NetPlayer`, create a permission worker:

```cs
public sealed class MyPermissionsWorker : IPermissionWorker<NetPlayer>
{
    public static List<string> DefaultPermissions = new List<string>()
    {
        "perm1", "perm2", "test"
    };

    public PermissionAccess HasPermission(NetPlayer target, string permission)
    {
        // If the permission is "MyPermission", this extension blocks it entirely
        // Other extensions cannot override this block
        if (permission == "permission_the_player_should_never_have")
            return PermissionAccess.Blocked;

        // Grant permission if listed in DefaultPermissions
        if (DefaultPermissions.Contains(permission))
            return PermissionAccess.HasPermission;

        // No match - defer to other extensions
        return PermissionAccess.None;
    }

    // Other permission checks (deferred by default)
    public PermissionAccess HasChestPermission(NetPlayer target, int x, int y) => PermissionAccess.None;
    public PermissionAccess HasChestEditPermission(NetPlayer target, int x, int y) => PermissionAccess.None;
    public PermissionAccess HasSignPermission(NetPlayer target, int x, int y) => PermissionAccess.None;
    public PermissionAccess HasSignEditPermission(NetPlayer target, int x, int y) => PermissionAccess.None;
    public PermissionAccess HasTEPermission(NetPlayer target, int x, int y) => PermissionAccess.None;
    public PermissionAccess HasTilePermission(NetPlayer target, int x, int y, int? width = null, int? height = null) => PermissionAccess.None;
}
```

Register the worker with the permission node (`PermissionNode<T>`). For `NetPlayer`, use `AmethystSession.PlayerPermissions`:

```cs
IPermissionWorker<NetPlayer> worker = new MyPermissionsWorker();

// Registration
AmethystSession.PlayerPermissions.Register(worker);

// Deregistration (required for plugin cleanup)
AmethystSession.PlayerPermissions.Unregister(worker);
```