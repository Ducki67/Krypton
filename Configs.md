# Configs (Krypton)
Here's everything what to know about configuring this GS / about some configs

## General stuff
I kept ALL original Erbium config so here's what **I** added + i DID categorized all configs so its easier to read all that stuff ofcourse its very messy still but who cares.

---

***LateGame***\
**bLGV2** - A Loot controller between the OLD and NEW "versionized" Late Game (DONT TURN THIS OFF if ur runing version below 4.5!!)\ 
This can also be changed in teh GUI on the `LateGame` tab IN-GAME / IN REAL TIME!

---

***GUI Configs***\
Dont really touch these untill its perfect!!\

**bUsePlaylistTab** - This Enbales the Expermental `Playlist` tab.\ 
ALSO if you use so Comment out **static inline auto Playlist = L"";** at the top  so it works.

**bAnim** - this is the STATE of reset builds animation (idc if u set to `false`) + this used by the reset build command the button so just check before you chnage it

**bExploit** - Legal cheats lol (not even dont yet)\
**bEAuth** - ALWAYS BE TRUE if exploits are on (auth systen to see the tab even if ur giving out the gs)\
**bAuthed** - DO NOT chnage this noe metter what!!!\
**( This will use ur radmin IP so those are in Erbium's dllmain.cpp AKA the whitelist)**

---

***GS related stuff***\
This is like 50/50 if works for u

**bUseCustomShit** - DONT ENABLE THIS SHIT IF U DONT KNOW WHAT YOU ARE DOING!!!! (ALSO Check FortGameMode.cpp before yout use this!!)\
**CustomBusDef = L"/Game/Athena/Items/Cosmetics/BattleBuses/BBID_DefaultBus.BBID_DefaultBus";** - bus def
**CustomSupplyDropDef = L"/Game/Athena/SupplyDrops/AthenaSupplyDrop.AthenaSupplyDrop_C";** - default supply drop maybe\
Default: `/Game/Athena/SupplyDrops/AthenaSupplyDrop.AthenaSupplyDrop_C` or just dont use this custom feature

---

***Creative***\
oh boy oh boy Not so much but it is what it is

**bMagicMethodCreative** - magic cool fantastic ud creaative method Erbium 2025 -miko

**PlotDefinition = L"/Game/Playgrounds/Items/Plots/TheBlock_Season7";** - put any map / plot here (e.g. "/Game/Playgrounds/Items/Plots/TheBlock_Season7" The Block from S7)

---

***Client***\
Nothing here kinda

**bClientConsole67K** - not added but only realones know!
**bTestingforLG** - logging shit but removed the functionality of this

---


## Expermental playlist tab
How it works:


**`in Coniguration.h`**
```cpp

/// Very expermental

// Playlist selectorV2 |  used for GUI plz enable "bUsePlaylistTab" to use it!!
struct FPlaylist
{
    
    // This is the ONLY thing that matters
    static inline int PlaylistIndex = 0;

    // Playlist paths
    static inline const wchar_t* Playlists[] =
    {
        L"/Game/Athena/Playlists/Playlist_DefaultSolo.Playlist_DefaultSolo", // 0
        L"/Game/Athena/Playlists/Playlist_DefaultDuo.Playlist_DefaultDuo",   // 1
        L"/Game/Athena/Playlists/Creative/Playlist_PlaygroundV2.Playlist_PlaygroundV2", // 2
        L"/Game/Athena/Playlists/Playground/Playlist_Playground.Playlist_Playground",   // 3

        L"/Game/Athena/Playlists/Showdown/Playlist_ShowdownAlt_Solo.Playlist_ShowdownAlt_Solo", // 4
        L"/Game/Athena/Playlists/Showdown/Playlist_Showdown_Solo.Playlist_Showdown_Solo", // 5


        L"/Game/Athena/Playlists/gg/Playlist_Gg_Reverse.Playlist_Gg_Reverse" // 6
    };

    // Used by the GameServer
    static inline const wchar_t* Playlist = Playlists[PlaylistIndex];
};
```

**`in Gui.cpp`**
```cpp
// part1
if (FConfiguration::bUsePlaylistTab) // make sure to comment out all the "( static inline auto Playlist L"Playlist_X"; )"  stuff to work
{
    if (gsStatus == NotReady)
    {
        if (ImGui::BeginTabItem("Playlist"))
        {
            SelectedUI = 2;
            ImGui::EndTabItem();
        }
    }
}
```

```cpp
// part2
case 2: // test1 tab - ducki67
{

    ImGui::Text("PLAYLIST OPTIONS");

    if (ImGui::BeginTable("Table", 2))
    {

        ImGui::TableNextRow();

        ImGui::TableNextColumn();
        ImGui::Text("Playlist list");
        ImGui::BeginChild("Gamemodes", ImVec2(385, 200), true, ImGuiWindowFlags_HorizontalScrollbar);
        ImGui::Text("Gamemodes:");

        ImGui::RadioButton("BR Solo", &FPlaylist::PlaylistIndex, 0);
        ImGui::RadioButton("BR Duos", &FPlaylist::PlaylistIndex, 1);
        ImGui::RadioButton("Creative (7.20+)", &FPlaylist::PlaylistIndex, 2);
        ImGui::RadioButton("Playground (4.5 - 10.40)", &FPlaylist::PlaylistIndex, 3);
        ImGui::RadioButton("Arena Solo (8.51+)", &FPlaylist::PlaylistIndex, 4);
        ImGui::RadioButton("Tournament Solo", &FPlaylist::PlaylistIndex, 5);

        ImGui::RadioButton("Arsenal (Gun Game)", &FPlaylist::PlaylistIndex, 6);

        // Update active playlist
        FPlaylist::Playlist = FPlaylist::Playlists[FPlaylist::PlaylistIndex];

        ImGui::EndChild();


        ImGui::TableNextColumn();
        ImGui::Text("Custom Playlist settings");
        ImGui::BeginChild("CustomPlaylistsettingsChild", ImVec2(375, 200), true, ImGuiWindowFlags_HorizontalScrollbar);
        ImGui::Text("Settings will be here...");

        ImGui::Text("Uhm idfk skidda");


        ImGui::EndChild();


        ImGui::EndTable();
    }

    ImGui::Separator();
    ImGui::Spacing();
    ImGui::Spacing();
    ImGui::Text("Playlist Path: %ls", FPlaylist::Playlist);
    ImGui::Spacing();
    ImGui::Separator();

    break;
}
```


