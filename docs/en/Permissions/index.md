# Permission Interfaces

Amethyst allows plugins/modules to manage player permissions via `IPermissionWorker<T>` and `PermissionsNode<T>`.

- `IPermissionable`: Base interface for permission-enabled objects (e.g., `NetPlayer`, `ConsoleSender`)
- `PermissionsNode<T>`: Node for coordinating permission checks across extensions

## The IPermissionable Interface

`IPermissionable` objects must implement permission checks. Example from `NetPlayer`:

```cs
public sealed class NetPlayer : IPermissionable
{
    public bool HasPermission(string permission)
        => IsCapable && (IsRootGranted || AmethystSession.PlayerPermissions.HandleResult(p => p.HasPermission(this, permission)) == PermissionAccess.HasPermission);

    // Other permission check methods follow the same pattern:
    public bool HasChestPermission(int x, int y) => /* ... */;
    public bool HasChestEditPermission(int x, int y) => /* ... */;
    public bool HasSignPermission(int x, int y) => /* ... */;
    public bool HasSignEditPermission(int x, int y) => /* ... */;
    public bool HasTEPermission(int x, int y) => /* ... */;
    public bool HasTilePermission(int x, int y, int? width = null, int? height = null) => /* ... */;
}
```

Players with `IsRootGranted` (enabled via `/grantroot` in debug mode) bypass all permission checks.

## PermissionNode<T> Internals

Excerpt from `PermissionsNode<T>`:

```cs
public class PermissionsNode<T> where T : IPermissionable
{
    internal List<IPermissionWorker<T>> Workers = new List<IPermissionWorker<T>>();

    internal PermissionAccess HandleResult(Func<IPermissionWorker<T>, PermissionAccess> invokeFunc)
    {
        PermissionAccess access = PermissionAccess.None;
        foreach (var worker in Workers)
        {
            var result = invokeFunc(worker);
            if (result == PermissionAccess.HasPermission) access = result;
            if (result == PermissionAccess.Blocked) return result; // Immediate block
        }
        return access;
    }
}
```