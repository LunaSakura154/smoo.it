<template>
  <smoo-card
    class="smoo-view-host-commands"
    :visible="$attrs.visible"
    accordion="host"
    key="commands"
    @show="$emit('show', $event)"
    @hidden="$emit('hidden', $event)"
  >
    <template #header>Commands to control or interact with a running server</template>

    <div>
      When running the server with the <a-int name="host" id="binary">binary</a-int>,
      attaching to the <a-int name="host" id="docker">docker</a-int> container,
      or using the <a-int name="host" id="discord">Discord</a-int> integration,
      you can send it commands to control and interact with it.
      Following are all the commands that the server version <code>1.0.5</code> understands.
    </div>

    <div>
      <kbd>help</kbd>
      Outputs a list of all possible commands.
    </div>

    <div>
      <kbd>list</kbd>
      Outputs the player names and profile IDs of all currently connected players.
    </div>

    <div>
      <kbd>loadsettings</kbd>
      Reloads the modified <a-int name="host" id="settings">settings.json</a-int>
      in without restarting the server.
    </div>

    <div>
      <kbd>maxplayers &lt;player-count&gt;</kbd>
      Sets the amount of players that can be connected to the server at the same time.
    </div>

    <div>
      <kbd>scenario {true|false}</kbd>
      Enables or disables the scenario merging.
      See the <a-int name="host" id="settings">settings</a-int> section for details.
    </div>

    <div>
      <kbd>sendall &lt;stage&gt;</kbd>
      Shortcut that will send all connected players to the given stage.
      <b-icon icon="info-circle-fill" id="commands-sendall"/>
      <b-tooltip target="commands-sendall" triggers="hover">
        This is identical to:
        <br/>
        <kbd>send &lt;stage&gt; "" -1 *</kbd>
      </b-tooltip>
      <br/>
      <ul>
        <li>
          <kbd>&lt;stage&gt;</kbd>
          <b-btn variant="info" size="sm" class="ml-2" style="padding: 0 0.4em;" v-b-modal.host-commands-stage-alias>show alias values</b-btn>
          <b-btn variant="info" size="sm" class="ml-2" style="padding: 0 0.4em;" v-b-modal.host-commands-stage-known>show known values</b-btn>
          <br/>Invalid stage values (e.g. for custom kingdoms) can be send by appending a <kbd>!</kbd> to the end.
        </li>
      </ul>
    </div>

    <div>
      <kbd>send &lt;stage&gt; &lt;warp-id&gt; &lt;scenario&gt; &lt;player&gt;...</kbd>
      Sends individual player(s) to a specific stage.
      <br/>
      <ul>
        <li>
          <kbd>&lt;stage&gt;</kbd>
          <b-btn variant="info" size="sm" class="ml-2" style="padding: 0 0.4em;" v-b-modal.host-commands-stage-alias>show alias values</b-btn>
          <b-btn variant="info" size="sm" class="ml-2" style="padding: 0 0.4em;" v-b-modal.host-commands-stage-known>show known values</b-btn>
          <br/>Invalid stage values (e.g. for custom kingdoms) can be send by appending a <kbd>!</kbd> to the end.
        </li>
        <li>
          <kbd>&lt;warp-id&gt;</kbd>
          <code>change stage ID</code> for the warp.
          To specify exactly where to spawn inside the <code>stage</code>.
          Use <kbd>""</kbd> for the default spawn point (the Odyssey).
        </li>
        <li>
          <kbd>&lt;scenario&gt;</kbd>
          <b-btn variant="info" size="sm" style="padding: 0 0.4em;" v-b-modal.host-commands-scenarios>show known values</b-btn>
          What scenario to send the players to. Has to be an integer between <kbd>-1</kbd> and <kbd>127</kbd>.
          Use <kbd>-1</kbd> to keep the scenario unchanged.
        </li>
        <li>
          <kbd>&lt;player&gt;...</kbd>
          A list of players to send (separated by spaces).
          Can be their player name or their profile ID.
          <kbd>*</kbd> is also a valid value to send all connected players.
        </li>
      </ul>
    </div>

    <div>
      <kbd>rejoin &lt;player-name&gt;...</kbd>
      Closes the connection to the client of the player(s).
      The client will automatically reconnect again.
      <br/>
      <ul>
        <li>
          <kbd>&lt;player-name&gt;...</kbd>
          A list of player names to rejoin (separated by spaces).
          <kbd>*</kbd> is also a valid value for all connected players.
          If you start the list with <kbd>!*</kbd>, it will rejoin all but the following players.
        </li>
      </ul>
    </div>

    <div>
      <kbd>crash &lt;player-name&gt;...</kbd>
      Send the player(s) to an invalid stage that will crash the game,
      and then closes the connection to the client of the player(s).
      <br/>
      <ul>
        <li>
          <kbd>&lt;player-name&gt;...</kbd>
          A list of player names to crash (separated by spaces).
          <kbd>*</kbd> is also a valid value for all connected players.
          If you start the list with <kbd>!*</kbd>, it will crash all but the following players.
        </li>
      </ul>
    </div>

    <div>
      <kbd>ban ...</kbd>
      Commands to control who or what is banned on this server by adding it to the <code>banlists</code> (see <a-int name="host" id="settings">settings</a-int>).
      <div class="sub-commands">

        <div>
          <kbd>ban list</kbd>
          <b-badge variant="primary" v-b-tooltip.html="'This command was added with server version <code>1.0.4</code>.'">1.0.4</b-badge>
          Shows all entries on the <code>banlists</code> and whether they are enabled or not.
        </div>

        <div>
          <kbd>ban {enable|disable}</kbd>
          <b-badge variant="primary" v-b-tooltip.html="'This command was added with server version <code>1.0.4</code>.'">1.0.4</b-badge>
          Enables or disables all <code>banlists</code> at once.
        </div>

        <div>
          <kbd>ban player &lt;player-name&gt;...</kbd>
          <b-badge variant="secondary" v-b-tooltip.html="'Before server version <code>1.0.4</code> this command was just <kbd>ban &amp;lt;player-name&amp;gt;...</kbd>'">1.0.4</b-badge>
          Sends the connected player(s) to an invalid stage that will crash the game,
          adds their profile IDs and IPv4 addresses to the <code>banlists</code>,
          and then ignores the connection to the client of the player(s).
          <br/>
          <ul>
            <li>
              <kbd>&lt;player-name&gt;...</kbd>
              A list of player names to ban (separated by spaces).
              <kbd>*</kbd> is also a valid value for all connected players.
              If you start the list with <kbd>!*</kbd>, it will ban all but the following players.
            </li>
          </ul>
        </div>

        <div>
          <kbd>ban profile &lt;profile-id&gt;</kbd>
          <b-badge variant="primary" v-b-tooltip.html="'This command was added with server version <code>1.0.4</code>.'">1.0.4</b-badge>
          Adds the profile ID to the <code>banlist</code>.
          If a player with that profile ID is connected, then it sends them to an invalid stage that will crash the game,
          and then ignores the connection to the client of the player.
          <br/>
          <ul>
            <li><kbd>&lt;profile-id&gt;</kbd> The unique profile ID of the player (not its name) that should be banned from the server.</li>
          </ul>
        </div>

        <div>
          <kbd>ban ip &lt;ipv4-address&gt;</kbd>
          <b-badge variant="primary" v-b-tooltip.html="'This command was added with server version <code>1.0.4</code>.'">1.0.4</b-badge>
          Adds the IPv4 address to the <code>banlist</code>.
          If a player with that IPv4 address is connected, then it sends them to an invalid stage that will crash the game,
          and then ignores the connection to the client of the player.
          <br/>
          <ul>
            <li><kbd>&lt;ipv4-address&gt;</kbd> The IPv4 address that should be banned from the server.</li>
          </ul>
        </div>

        <div>
          <kbd>ban stage &lt;stage&gt;</kbd>
          <b-badge variant="primary" v-b-tooltip.html="'This command was added with server version <code>1.0.4</code>.'">1.0.4</b-badge>
          Adds the stage to the <code>banlist</code>.
          If a player enters a banned stage, then it sends them to an invalid stage that will crash the game,
          and then ignores the connection to the client of the player.
          <br/>
          <ul>
            <li>
              <kbd>&lt;stage&gt;</kbd>
              <b-btn variant="info" size="sm" class="ml-2" style="padding: 0 0.4em;" v-b-modal.host-commands-stage-alias>show alias values</b-btn>
              <b-btn variant="info" size="sm" class="ml-2" style="padding: 0 0.4em;" v-b-modal.host-commands-stage-known>show known values</b-btn>
              <br/>Using an alias will add all known stages in that kingdom to the <code>banlist</code> individually.
              <br/>Invalid stage values (e.g. for custom kingdoms) can be added by appending a <kbd>!</kbd> to the end.
            </li>
          </ul>
        </div>

        <div>
          <kbd>ban gamemode &lt;game-mode&gt;</kbd>
          <b-badge variant="warning" class="mr-1" v-b-tooltip.html="'This command will be added with server version <code>1.0.5</code>.'">1.0.5</b-badge>
          Adds game modes to the <code>banlist</code>.
          If a player enters a banned game mode, then it sends them to an invalid stage that will crash the game,
          and then ignores the connection to the client of the player.
          <br/>
          <ul>
            <li>
              <kbd>&lt;game-mode&gt;</kbd>
              The index that the mod uses for the game mode or its identifier.
              <ul>
               <li><kbd>-1</kbd> or <kbd>None</kbd>: No active game mode.</li>
               <li><kbd>0</kbd> or <kbd>Legacy</kbd>: Used by older mod versions from before the game mode was synchronized.</li>
               <li><kbd>1</kbd> or <kbd>HideAndSeek</kbd>: <a-int name="play" id="hide-and-seek">Hide &amp; Seek</a-int></li>
               <li><kbd>2</kbd> or <kbd>Sardines</kbd>: Sardines</li>
               <li><kbd>3</kbd> or <kbd>FreezeTag</kbd>: Freeze-Tag</li>
              </ul>
            </li>
          </ul>
        </div>

      </div>
    </div>

    <div>
      <kbd>unban ...</kbd>
      Commands to remove entries from the <code>banlists</code> (see <a-int name="host" id="settings">settings</a-int>).
      <div class="sub-commands">

        <div>
          <kbd>unban profile &lt;profile-id&gt;</kbd>
          <b-badge variant="primary" v-b-tooltip.html="'This command was added with server version <code>1.0.4</code>.'">1.0.4</b-badge>
          Removes the profile ID from the <code>banlist</code>.
          <br/>
          <ul>
            <li><kbd>&lt;profile-id&gt;</kbd> The unique profile ID of the player (not its name) that should no longer be banned from the server.</li>
          </ul>
        </div>

        <div>
          <kbd>unban ip &lt;ipv4-address&gt;</kbd>
          <b-badge variant="primary" v-b-tooltip.html="'This command was added with server version <code>1.0.4</code>.'">1.0.4</b-badge>
          Removes the IPv4 address from the <code>banlist</code>.
          <br/>
          <ul>
            <li><kbd>&lt;ipv4-address&gt;</kbd> The IPv4 address that should no longer be banned from the server.</li>
          </ul>
        </div>

        <div>
          <kbd>unban stage &lt;stage&gt;</kbd>
          <b-badge variant="primary" v-b-tooltip.html="'This command was added with server version <code>1.0.4</code>.'">1.0.4</b-badge>
          Removes the stage from the <code>banlist</code>.
          <br/>
          <ul>
            <li>
              <kbd>&lt;stage&gt;</kbd>
              <b-btn variant="info" size="sm" class="ml-2" style="padding: 0 0.4em;" v-b-modal.host-commands-stage-alias>show alias values</b-btn>
              <b-btn variant="info" size="sm" class="ml-2" style="padding: 0 0.4em;" v-b-modal.host-commands-stage-known>show known values</b-btn>
              <br/>Using an alias will remove all known stages in that kingdom from the <code>banlist</code> individually.
              <br/>Invalid stage values (e.g. for custom kingdoms) can be removed by appending a <kbd>!</kbd> to the end.
            </li>
          </ul>
        </div>

        <div>
          <kbd>unban gamemode &lt;game-mode&gt;</kbd>
          <b-badge variant="warning" class="mr-1" v-b-tooltip.html="'This command will be added with server version <code>1.0.5</code>.'">1.0.5</b-badge>
          Removes the game mode from the <code>banlist</code>.
          <br/>
          <ul>
            <li>
              <kbd>&lt;game-mode&gt;</kbd>
              The index that the mod uses for the game mode or its identifier.
              <ul>
               <li><kbd>-1</kbd> or <kbd>None</kbd>: No active game mode.</li>
               <li><kbd>0</kbd> or <kbd>Legacy</kbd>: Used by older mod versions from before the game mode was synchronized.</li>
               <li><kbd>1</kbd> or <kbd>HideAndSeek</kbd>: <a-int name="play" id="hide-and-seek">Hide &amp; Seek</a-int></li>
               <li><kbd>2</kbd> or <kbd>Sardines</kbd>: Sardines</li>
               <li><kbd>3</kbd> or <kbd>FreezeTag</kbd>: Freeze-Tag</li>
              </ul>
            </li>
          </ul>
        </div>

      </div>
    </div>

    <div>
      <kbd>tag ...</kbd>
      Hide &amp; Seek helper commands.
      <div class="sub-commands">

        <div>
          <kbd>tag time &lt;player-name&gt; &lt;minutes&gt; &lt;seconds&gt;</kbd>
          Manually sets the Hide &amp; Seek timer for a player.
          <br/>
          <ul>
            <li><kbd>&lt;player-name&gt;</kbd> One specific player name or <kbd>*</kbd> for all connected players.</li>
            <li><kbd>&lt;minutes&gt;</kbd> Value between <kbd>0</kbd> and <kbd>65535</kbd>.</li>
            <li><kbd>&lt;seconds&gt;</kbd> Value between <kbd>0</kbd> and <kbd>59</kbd>.</li>
          </ul>
        </div>

        <div>
          <kbd>tag seeking &lt;player-name&gt; {true|false}</kbd>
          Manually sets the Hide &amp; Seek status for a player to seeker (<code>true</code>) or hider (<code>false</code>).
          <br/>
          <ul>
            <li><kbd>&lt;player-name&gt;</kbd> One specific player name or <kbd>*</kbd> for all connected players.</li>
          </ul>
        </div>

        <div>
          <kbd>tag start &lt;seconds&gt; &lt;player-name&gt;...</kbd>
          Starts a countdown that will set the Hide &amp; Seek status of all connected players when the time is over.
          <br/>
          <ul>
            <li><kbd>&lt;seconds&gt;</kbd> Value between <kbd>0</kbd> and <kbd>255</kbd>.</li>
            <li><kbd>&lt;player-name&gt;...</kbd> A list of player names that will be seekers. Everyone else will become hiders.</li>
          </ul>
        </div>

      </div>
    </div>

    <div>
      <kbd>flip ...</kbd>
      Commands to control the <code>flip</code> feature (see <a-int name="host" id="settings">settings</a-int>).
      <div class="sub-commands">

        <div>
          <kbd>flip list</kbd>
          Lists all profile IDs that are on the flip list.
        </div>

        <div>
          <kbd>flip set {true|false}</kbd>
          Enables or disables the <code>flip</code> feature.
        </div>

        <div>
          <kbd>flip pov {both|self|others}</kbd>
          Sets the <a-int name="host" id="settings">setting</a-int> for who sees whom as flipped.
        </div>

        <div>
          <kbd>flip add &lt;profile-id&gt;</kbd>
          Adds a player to the flip list.
          <br/>
          <ul>
            <li><kbd>&lt;profile-id&gt;</kbd> The unique profile ID of the player (not its name) that should be added to the flip list.</li>
          </ul>
        </div>

        <div>
          <kbd>flip remove &lt;profile-id&gt;</kbd>
          Removes a player from the flip list.
          <br/>
          <ul>
            <li><kbd>&lt;profile-id&gt;</kbd> The unique profile ID of the player (not its name) that should be added to the flip list.</li>
          </ul>
        </div>

      </div>
    </div>

    <div>
      <kbd>shine ...</kbd>
      commands to manipulate shines (moons).
      <div class="sub-commands">

        <div>
          <kbd>shine list</kbd>
          Lists all shine IDs that have been collected globally by all players.
        </div>

        <div>
          <kbd>shine clear</kbd>
          Removes all saved shines on the server.
          This will not remove shines in-game from connected players.
        </div>

        <div>
          <kbd>shine sync</kbd>
          Manually syncs all saved shines to all connected players.
          Only those shines that are missing for the player from the point of view of the server are transferred.
          This will not remove shines in-game from connected players.
          This should normally happen automatically when entering the game, when there's a change to the shines and every two minutes.
        </div>

        <div>
          <kbd>shine send &lt;shine-id&gt; &lt;player-name&gt;...</kbd>
          Manually sends one shine to players.
          <br/>
          <ul>
            <li>
              <kbd>&lt;shine-id&gt;</kbd>
              The unique ID for an individual shine (moon) in the game.
              <b-btn variant="info" size="sm" class="ml-2" style="padding: 0 0.4em;" v-b-modal.host-commands-shine-id>show known values</b-btn>
            </li>
            <li>
              <kbd>&lt;player-name&gt;...</kbd>
              A list of player names that shall receive the shine (separated by spaces).
              <kbd>*</kbd> is also a valid value for all connected players.
            </li>
          </ul>
        </div>

        <div>
          <kbd>shine set {true|false}</kbd>
          <b-badge variant="primary" v-b-tooltip.html="'This command was added with server version <code>1.0.3</code>.'">1.0.3</b-badge>
          To enable or disable shine syncing.
        </div>

        <div>
          <kbd>shine exclude &lt;shine-id&gt;</kbd>
          <b-badge variant="warning" class="mr-1" v-b-tooltip.html="'This command will be added with server version <code>1.0.5</code>.'">1.0.5</b-badge>
          To prevent a moon to be synchronized with other players.
          <br/>
          <ul>
            <li>
              <kbd>&lt;shine-id&gt;</kbd>
              The unique ID for an individual shine (moon) in the game.
              <b-btn variant="info" size="sm" class="ml-2" style="padding: 0 0.4em;" v-b-modal.host-commands-shine-id>show known values</b-btn>
            </li>
          </ul>
        </div>

        <div>
          <kbd>shine include &lt;shine-id&gt;</kbd>
          <b-badge variant="warning" class="mr-1" v-b-tooltip.html="'This command will be added with server version <code>1.0.5</code>.'">1.0.5</b-badge>
          To no longer prevent a moon to be synchronized with other players.
          <br/>
          <ul>
            <li>
              <kbd>&lt;shine-id&gt;</kbd>
              The unique ID for an individual shine (moon) in the game.
              <b-btn variant="info" size="sm" class="ml-2" style="padding: 0 0.4em;" v-b-modal.host-commands-shine-id>show known values</b-btn>
            </li>
          </ul>
        </div>

      </div>
    </div>

    <div>
      <kbd>dscrestart</kbd>
      <b-badge variant="primary" v-b-tooltip.html="'This command was added with server version <code>1.0.2</code>.'">1.0.2</b-badge>
      Restarts the <a-int name="host" id="discord">Discord</a-int> bot.
    </div>

    <div>
      <kbd>restartserver</kbd>
      <b-badge variant="primary" v-b-tooltip.html="'This command was added with server version <code>1.0.3</code>.'">1.0.3</b-badge>
      Stops the server and restarts it again in a new process.
      This doesn't work well with <a-int name="host" id="docker">docker</a-int>,
      because when its main process stops, the whole docker container will be stopped.
    </div>

    <div>
      <kbd>exit</kbd> <kbd>quit</kbd> <kbd>q</kbd>
      Stops the server.
    </div>

    <stages-alias-modal id="host-commands-stage-alias"/>
    <stages-known-modal id="host-commands-stage-known"/>
    <scenarios-modal id="host-commands-scenarios"/>
    <moons-modal id="host-commands-shine-id"/>
  </smoo-card>
</template>

<style lang="scss">
.smoo-view-host-commands {
  kbd {
    display: inline-block;
    margin: 0.2rem;
    &:first-child {
      margin-left: 0;
    }
  }
  ul > li > kbd {
    background-color: rgba(0, 0, 0, 0.5);
  }
  .card-body > div, .card-body .sub-commands > div {
    &:not(:first-child) {
      padding-top: 1rem;
      border-top: 1px solid rgba(0, 0, 0, 0.05);
    }
    &:not(:last-child) {
      padding-bottom: 1rem;
    }
  }
  .sub-commands {
    margin-top: 1rem;
    & > div {
      margin-left: 1.5rem;
    }
  }
}
</style>

<script lang="ts" src="./commands.ts"/>
