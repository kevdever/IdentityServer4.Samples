to initialize the three contexts and have the migrations added in the same structure as specified below, you need to first run three init commands:

Desired structure of Data:

Data
--ApplicationDbContext.cs
--Migrations
--|
----<ApplicationDbContext - initial migration>
----<ApplicationDbContext - snapshot>
----IdentityServer
------|
------ConfigurationDb
--------|
--------<initial migration>
--------<snapshot>
------PersistedGrantDb
--------|
--------<initial migration>
--------<snapshot>

Using the a command prompt (not package manager), run the following:

> dotnet ef migrations add CreateIdentitySchema --context ApplicationDbContext -o Data/Migrations

> dotnet ef migrations add InitialIdentityServerConfigurationDbMigration --context ConfigurationDbContext -o Data/Migrations/IdentityServer/ConfigurationDb

> dotnet ef migrations add InitialIdentityServerPersistedGrantDbMigration --context PersistedGrantDbContext -o Data/Migrations/IdentityServer/PersistedGrantDb



Regarding seeding, Program.Main requires cli arguments be passed in "/seed" to initiate the seeding.  That command has been baked into the project debug settings.