// ==UserScript==
// @name         PoeTrade Item Copy
// @namespace    http://tampermonkey.net/
// @version      1.7
// @description  Copy items from Poe.Trade to Path of Building
// @author       Fikal
// @match        http://poe.trade/*
// @require      http://ajax.googleapis.com/ajax/libs/jquery/2.1.0/jquery.min.js
// @require      https://gist.github.com/raw/2625891/waitForKeyElements.js
// @require      https://cdnjs.cloudflare.com/ajax/libs/clipboard.js/1.6.0/clipboard.min.js
// ==/UserScript==

waitForKeyElements ("#search-results-0", addCopyLinks);


function addCopyLinks() {
    var rows = $('*[id^="item-container-"]');
    
    rows.each(function(){

        var that = $(this),
            id = $(this).attr('id').match(/[0-9]+/),
            link = $('<a>').attr('id', id[0]).addClass('copy2POB').text('Copy');

        link.click(function(){
            var poeObject = new PoeItem(that);
            var currentLink = $(this);
            
            var clipboard = new Clipboard('.copy2POB', {
                text: function(trigger) {
                    return poeObject.buildItem();
                }
            });
            
            clipboard.on('success', function(e) {
                $(currentLink).text('Copied to clipboard');
            });
            
       });

        $(this).find(".bottom-row .proplist").append($('<li>').html(link));
        
       

    });
}

function PoeItem(div) {
    
    var itemName = "";
    var itemType = "";
        
    this.htmlDiv = div;
    //taken from poe.trade source
    this.items_types = ["Aventail Helmet", "Barbute Helmet", "Battered Helm", "Bone Circlet", "Bone Helmet", "Callous Mask", "Close Helmet", "Cone Helmet", "Crusader Helmet", "Deicide Mask", "Eternal Burgonet", "Ezomyte Burgonet", "Fencer Helm", "Festival Mask", "Fluted Bascinet", "Gilded Sallet", "Gladiator Helmet", "Golden Mask", "Golden Wreath", "Great Crown", "Great Helmet", "Harlequin Mask", "Hubris Circlet", "Hunter Hood", "Iron Circlet", "Iron Hat", "Iron Mask", "Lacquered Helmet", "Leather Cap", "Leather Hood", "Lion Pelt", "Lunaris Circlet", "Magistrate Crown", "Mind Cage", "Necromancer Circlet", "Nightmare Bascinet", "Noble Tricorne", "Pig-Faced Bascinet", "Plague Mask", "Praetor Crown", "Prophet Crown", "Raven Mask", "Reaver Helmet", "Regicide Mask", "Royal Burgonet", "Rusted Coif", "Sallet", "Samite Helmet", "Scare Mask", "Secutor Helm", "Siege Helmet", "Silken Hood", "Sinner Tricorne", "Solaris Circlet", "Soldier Helmet", "Steel Circlet", "Torture Cage", "Tribal Circlet", "Tricorne", "Ursine Pelt", "Vaal Mask", "Vine Circlet", "Visored Sallet", "Wolf Pelt", "Zealot Helmet", "Arming Axe", "Boarding Axe", "Broad Axe", "Butcher Axe", "Ceremonial Axe", "Chest Splitter", "Cleaver", "Decorative Axe", "Engraved Hatchet", "Etched Hatchet", "Infernal Axe", "Jade Hatchet", "Jasper Axe", "Karui Axe", "Reaver Axe", "Royal Axe", "Runic Hatchet", "Rusted Hatchet", "Siege Axe", "Spectral Axe", "Tomahawk", "Vaal Hatchet", "War Axe", "Wraith Axe", "Wrist Chopper", "Amethyst Flask", "Aquamarine Flask", "Basalt Flask", "Bismuth Flask", "Colossal Hybrid Flask", "Colossal Life Flask", "Colossal Mana Flask", "Diamond Flask", "Divine Life Flask", "Divine Mana Flask", "Eternal Life Flask", "Eternal Mana Flask", "Giant Life Flask", "Giant Mana Flask", "Grand Life Flask", "Grand Mana Flask", "Granite Flask", "Greater Life Flask", "Greater Mana Flask", "Hallowed Hybrid Flask", "Hallowed Life Flask", "Hallowed Mana Flask", "Jade Flask", "Large Hybrid Flask", "Large Life Flask", "Large Mana Flask", "Medium Hybrid Flask", "Medium Life Flask", "Medium Mana Flask", "Quartz Flask", "Quicksilver Flask", "Ruby Flask", "Sacred Hybrid Flask", "Sacred Life Flask", "Sacred Mana Flask", "Sanctified Life Flask", "Sanctified Mana Flask", "Sapphire Flask", "Silver Flask", "Small Hybrid Flask", "Small Life Flask", "Small Mana Flask", "Stibnite Flask", "Sulphur Flask", "Topaz Flask", "Fishing Rod", "Ancient Sword", "Antique Rapier", "Apex Rapier", "Baselard", "Basket Rapier", "Battered Foil", "Battle Sword", "Broad Sword", "Burnished Foil", "Copper Sword", "Corsair Sword", "Courtesan Sword", "Cutlass", "Dragonbone Rapier", "Dragoon Sword", "Dusk Blade", "Elder Sword", "Elegant Foil", "Elegant Sword", "Estoc", "Eternal Sword", "Fancy Foil", "Gemstone Sword", "Gladius", "Graceful Sword", "Grappler", "Harpy Rapier", "Hook Sword", "Jagged Foil", "Jewelled Foil", "Legion Sword", "Midnight Blade", "Pecoraro", "Primeval Rapier", "Rusted Spike", "Rusted Sword", "Sabre", "Serrated Foil", "Smallsword", "Spiraled Foil", "Tempered Foil", "Thorn Rapier", "Tiger Hook", "Twilight Blade", "Vaal Blade", "Vaal Rapier", "Variscite Blade", "War Sword", "Whalebone Rapier", "Wyrmbone Rapier", "Awl", "Blinder", "Cat's Paw", "Double Claw", "Eagle Claw", "Eye Gouger", "Fright Claw", "Gemini Claw", "Gouger", "Great White Claw", "Gut Ripper", "Hellion's Paw", "Imperial Claw", "Nailed Fist", "Noble Claw", "Prehistoric Claw", "Sharktooth Claw", "Sparkling Claw", "Terror Claw", "Thresher Claw", "Throat Stabber", "Tiger's Paw", "Timeworn Claw", "Twin Claw", "Vaal Claw", "Arena Plate", "Assassin's Garb", "Astral Plate", "Battle Lamellar", "Battle Plate", "Blood Raiment", "Bone Armour", "Bronze Plate", "Buckskin Tunic", "Cabalist Regalia", "Carnal Armour", "Chain Hauberk", "Chainmail Doublet", "Chainmail Tunic", "Chainmail Vest", "Chestplate", "Colosseum Plate", "Commander's Brigandine", "Conjurer's Vestment", "Conquest Chainmail", "Copper Plate", "Coronal Leather", "Crimson Raiment", "Crusader Chainmail", "Crusader Plate", "Crypt Armour", "Cutthroat's Garb", "Desert Brigandine", "Destiny Leather", "Destroyer Regalia", "Devout Chainmail", "Dragonscale Doublet", "Eelskin Tunic", "Elegant Ringmail", "Exquisite Leather", "Field Lamellar", "Frontier Leather", "Full Chainmail", "Full Dragonscale", "Full Leather", "Full Plate", "Full Ringmail", "Full Scale Armour", "Full Wyrmscale", "General's Brigandine", "Gladiator Plate", "Glorious Leather", "Glorious Plate", "Golden Mantle", "Golden Plate", "Holy Chainmail", "Hussar Brigandine", "Infantry Brigandine", "Lacquered Garb", "Latticed Ringmail", "Light Brigandine", "Lordly Plate", "Loricated Ringmail", "Mage's Vestment", "Majestic Plate", "Necromancer Silks", "Occultist's Vestment", "Oiled Coat", "Oiled Vest", "Ornate Ringmail", "Padded Jacket", "Padded Vest", "Plate Vest", "Quilted Jacket", "Ringmail Coat", "Sacrificial Garb", "Sadist Garb", "Sage's Robe", "Saint's Hauberk", "Saintly Chainmail", "Savant's Robe", "Scale Doublet", "Scale Vest", "Scarlet Raiment", "Scholar's Robe", "Sentinel Jacket", "Shabby Jerkin", "Sharkskin Tunic", "Silk Robe", "Silken Garb", "Silken Vest", "Silken Wrap", "Simple Robe", "Sleek Coat", "Soldier's Brigandine", "Spidersilk Robe", "Strapped Leather", "Sun Leather", "Sun Plate", "Thief's Garb", "Triumphant Lamellar", "Vaal Regalia", "Varnished Coat", "War Plate", "Waxed Garb", "Widowsilk Robe", "Wild Leather", "Wyrmscale Doublet", "Zodiac Leather", "Ancestral Club", "Auric Mace", "Barbed Club", "Battle Hammer", "Behemoth Mace", "Bladed Mace", "Ceremonial Mace", "Dragon Mace", "Dream Mace", "Driftwood Club", "Flanged Mace", "Gavel", "Legion Hammer", "Nightmare Mace", "Ornate Mace", "Pernarch", "Petrified Club", "Phantom Mace", "Rock Breaker", "Spiked Club", "Stone Hammer", "Tenderizer", "Tribal Club", "War Hammer", "Wyrm Mace", "Agate Amulet", "Amber Amulet", "Ashscale Talisman", "Avian Twins Talisman", "Black Maw Talisman", "Blue Pearl Amulet", "Bonespire Talisman", "Breakrib Talisman", "Chrysalis Talisman", "Citrine Amulet", "Clutching Talisman", "Coral Amulet", "Deadhand Talisman", "Deep One Talisman", "Fangjaw Talisman", "Gold Amulet", "Greatwolf Talisman", "Hexclaw Talisman", "Horned Talisman", "Jade Amulet", "Jet Amulet", "Jet Amulet", "Lapis Amulet", "Lone Antler Talisman", "Longtooth Talisman", "Mandible Talisman", "Marble Amulet", "Monkey Paw Talisman", "Monkey Twins Talisman", "Onyx Amulet", "Paua Amulet", "Primal Skull Talisman", "Rot Head Talisman", "Rotfeather Talisman", "Ruby Amulet", "Spinefuse Talisman", "Splitnewt Talisman", "Three Hands Talisman", "Three Rat Talisman", "Turquoise Amulet", "Undying Flesh Talisman", "Wereclaw Talisman", "Writhing Talisman", "Brass Maul", "Colossus Mallet", "Coronal Maul", "Dread Maul", "Driftwood Maul", "Fright Maul", "Great Mallet", "Imperial Maul", "Jagged Maul", "Karui Maul", "Mallet", "Meatgrinder", "Morning Star", "Piledriver", "Plated Maul", "Sledgehammer", "Solar Maul", "Spiny Maul", "Steelhead", "Terror Maul", "Totemic Maul", "Tribal Maul", "Abyssal Sceptre", "Blood Sceptre", "Bronze Sceptre", "Carnal Sceptre", "Crystal Sceptre", "Darkwood Sceptre", "Driftwood Sceptre", "Grinning Fetish", "Horned Sceptre", "Iron Sceptre", "Karui Sceptre", "Lead Sceptre", "Ochre Sceptre", "Opal Sceptre", "Platinum Sceptre", "Quartz Sceptre", "Ritual Sceptre", "Royal Sceptre", "Sambar Sceptre", "Sekhem", "Shadow Sceptre", "Stag Sceptre", "Tyrant's Sekhem", "Vaal Sceptre", "Void Sceptre", "Abyssal Axe", "Dagger Axe", "Despot Axe", "Double Axe", "Ezomyte Axe", "Fleshripper", "Gilded Axe", "Headsman Axe", "Jade Chopper", "Jasper Chopper", "Karui Chopper", "Labrys", "Noble Axe", "Poleaxe", "Shadow Axe", "Stone Axe", "Sundering Axe", "Talon Axe", "Timber Axe", "Vaal Axe", "Void Axe", "Woodsplitter", "Bastard Sword", "Butcher Sword", "Corroded Blade", "Curved Blade", "Engraved Greatsword", "Etched Greatsword", "Exquisite Blade", "Ezomyte Blade", "Footman Sword", "Headman's Sword", "Highland Blade", "Infernal Sword", "Lion Sword", "Lithe Blade", "Longsword", "Ornate Sword", "Reaver Sword", "Spectral Sword", "Tiger Sword", "Two-Handed Sword", "Vaal Greatsword", "Wraith Sword", "Cobalt Jewel", "Crimson Jewel", "Prismatic Jewel", "Viridian Jewel", "Assassin Bow", "Bone Bow", "Citadel Bow", "Composite Bow", "Compound Bow", "Crude Bow", "Death Bow", "Decimation Bow", "Decurve Bow", "Golden Flame", "Grove Bow", "Harbinger Bow", "Highborn Bow", "Imperial Bow", "Ivory Bow", "Long Bow", "Maraketh Bow", "Ranger Bow", "Recurve Bow", "Reflex Bow", "Royal Bow", "Short Bow", "Sniper Bow", "Spine Bow", "Steelwood Bow", "Thicket Bow", "Ambush Mitts", "Ancient Gauntlets", "Antique Gauntlets", "Arcanist Gloves", "Assassin's Mitts", "Bronze Gauntlets", "Bronzescale Gauntlets", "Carnal Mitts", "Chain Gloves", "Clasped Mitts", "Conjurer Gloves", "Crusader Gloves", "Deerskin Gloves", "Dragonscale Gauntlets", "Eelskin Gloves", "Embroidered Gloves", "Fingerless Silk Gloves", "Fishscale Gauntlets", "Goathide Gloves", "Golden Bracers", "Goliath Gauntlets", "Gripped Gloves", "Hydrascale Gauntlets", "Iron Gauntlets", "Ironscale Gauntlets", "Legion Gloves", "Mesh Gloves", "Murder Mitts", "Nubuck Gloves", "Plated Gauntlets", "Rawhide Gloves", "Ringmail Gloves", "Riveted Gloves", "Samite Gloves", "Satin Gloves", "Serpentscale Gauntlets", "Shagreen Gloves", "Sharkskin Gloves", "Silk Gloves", "Slink Gloves", "Soldier Gloves", "Sorcerer Gloves", "Spiked Gloves", "Stealth Gloves", "Steel Gauntlets", "Steelscale Gauntlets", "Strapped Mitts", "Titan Gauntlets", "Trapper Mitts", "Vaal Gauntlets", "Velvet Gloves", "Wool Gloves", "Wrapped Mitts", "Wyrmscale Gauntlets", "Zealot Gloves", "Blunt Arrow Quiver", "Broadhead Arrow Quiver", "Conductive Quiver", "Cured Quiver", "Fire Arrow Quiver", "Heavy Quiver", "Light Quiver", "Penetrating Arrow Quiver", "Rugged Quiver", "Serrated Arrow Quiver", "Sharktooth Arrow Quiver", "Spike-Point Arrow Quiver", "Two-Point Arrow Quiver", "Alder Spiked Shield", "Alloyed Spiked Shield", "Ancient Spirit Shield", "Angelic Kite Shield", "Archon Kite Shield", "Baroque Round Shield", "Battle Buckler", "Bone Spirit Shield", "Branded Kite Shield", "Brass Spirit Shield", "Bronze Tower Shield", "Buckskin Tower Shield", "Burnished Spiked Shield", "Cardinal Round Shield", "Cedar Tower Shield", "Ceremonial Kite Shield", "Champion Kite Shield", "Chiming Spirit Shield", "Colossal Tower Shield", "Compound Spiked Shield", "Copper Tower Shield", "Corroded Tower Shield", "Corrugated Buckler", "Crested Tower Shield", "Crimson Round Shield", "Crusader Buckler", "Driftwood Spiked Shield", "Ebony Tower Shield", "Elegant Round Shield", "Enameled Buckler", "Etched Kite Shield", "Ezomyte Spiked Shield", "Ezomyte Tower Shield", "Fir Round Shield", "Fossilised Spirit Shield", "Gilded Buckler", "Girded Tower Shield", "Goathide Buckler", "Golden Buckler", "Hammered Buckler", "Harmonic Spirit Shield", "Imperial Buckler", "Ironwood Buckler", "Ivory Spirit Shield", "Jingling Spirit Shield", "Lacewood Spirit Shield", "Lacquered Buckler", "Laminated Kite Shield", "Layered Kite Shield", "Linden Kite Shield", "Mahogany Tower Shield", "Maple Round Shield", "Mirrored Spiked Shield", "Mosaic Kite Shield", "Oak Buckler", "Ornate Spiked Shield", "Painted Buckler", "Painted Tower Shield", "Pine Buckler", "Pinnacle Tower Shield", "Plank Kite Shield", "Polished Spiked Shield", "Rawhide Tower Shield", "Redwood Spiked Shield", "Reinforced Kite Shield", "Reinforced Tower Shield", "Rotted Round Shield", "Scarlet Round Shield", "Shagreen Tower Shield", "Sovereign Spiked Shield", "Spiked Bundle", "Spiked Round Shield", "Spiny Round Shield", "Splendid Round Shield", "Splintered Tower Shield", "Steel Kite Shield", "Studded Round Shield", "Supreme Spiked Shield", "Tarnished Spirit Shield", "Teak Round Shield", "Thorium Spirit Shield", "Titanium Spirit Shield", "Twig Spirit Shield", "Vaal Buckler", "Vaal Spirit Shield", "Walnut Spirit Shield", "War Buckler", "Yew Spirit Shield", "Ambusher", "Boot Blade", "Boot Knife", "Butcher Knife", "Carving Knife", "Copper Kris", "Demon Dagger", "Ezomyte Dagger", "Fiend Dagger", "Flaying Knife", "Glass Shank", "Golden Kris", "Gutting Knife", "Imp Dagger", "Imperial Skean", "Platinum Kris", "Poignard", "Prong Dagger", "Royal Skean", "Sai", "Skean", "Skinning Knife", "Slaughter Knife", "Stiletto", "Trisula", "Carved Wand", "Crystal Wand", "Demon's Horn", "Driftwood Wand", "Engraved Wand", "Faun's Horn", "Goat's Horn", "Heathen Wand", "Imbued Wand", "Omen Wand", "Opal Wand", "Pagan Wand", "Profane Wand", "Prophecy Wand", "Quartz Wand", "Sage Wand", "Serpent Wand", "Spiraled Wand", "Tornado Wand", "Ambush Boots", "Ancient Greaves", "Antique Greaves", "Arcanist Slippers", "Assassin's Boots", "Bronzescale Boots", "Carnal Boots", "Chain Boots", "Clasped Boots", "Conjurer Boots", "Crusader Boots", "Deerskin Boots", "Dragonscale Boots", "Eelskin Boots", "Goathide Boots", "Golden Caligae", "Goliath Greaves", "Hydrascale Boots", "Iron Greaves", "Ironscale Boots", "Leatherscale Boots", "Legion Boots", "Mesh Boots", "Murder Boots", "Nubuck Boots", "Plated Greaves", "Rawhide Boots", "Reinforced Greaves", "Ringmail Boots", "Riveted Boots", "Samite Slippers", "Satin Slippers", "Scholar Boots", "Serpentscale Boots", "Shackled Boots", "Shagreen Boots", "Sharkskin Boots", "Silk Slippers", "Slink Boots", "Soldier Boots", "Sorcerer Boots", "Stealth Boots", "Steel Greaves", "Steelscale Boots", "Strapped Boots", "Titan Greaves", "Trapper Boots", "Two-Toned Boots", "Vaal Greaves", "Velvet Slippers", "Wool Shoes", "Wrapped Boots", "Wyrmscale Boots", "Zealot Boots", "Amethyst Ring", "Breach Ring", "Coral Ring", "Diamond Ring", "Gold Ring", "Golden Hoop", "Iron Ring", "Moonstone Ring", "Opal Ring", "Paua Ring", "Prismatic Ring", "Ruby Ring", "Sapphire Ring", "Steel Ring", "Topaz Ring", "Two-Stone Ring", "Unset Ring", "Chain Belt", "Cloth Belt", "Crystal Belt", "Golden Obi", "Heavy Belt", "Leather Belt", "Rustic Sash", "Studded Belt", "Vanguard Belt", "Coiled Staff", "Crescent Staff", "Eclipse Staff", "Ezomyte Staff", "Foul Staff", "Gnarled Branch", "Highborn Staff", "Imperial Staff", "Iron Staff", "Judgement Staff", "Lathi", "Long Staff", "Maelstr\u00f6m Staff", "Military Staff", "Moon Staff", "Primitive Staff", "Primordial Staff", "Quarterstaff", "Royal Staff", "Serpentine Staff", "Vile Staff", "Woodful Staff"];
    
    this.item_rarities = ["Normal", "Magic", "Rare", "Unique", "Gem", "Currency", "", "", "", "Relic"];
    
    this.returnItemQuality = function() {
        return this.htmlDiv.find("td[data-name='q']").data("value");
    };
    
    this.returnImplicitCount = function() {
        
        var modCount = 0;
        this.htmlDiv.find(".withline li").each(function(){
            modCount += 1;
        });
        
        this.htmlDiv.find("ul .mods:not(.withline) li").each(function(){
            var that = $(this);
   
            if (that.text().indexOf('enchanted') !== -1) {
                modCount += 1;
            }
            if (that.text().indexOf('crafted') !== -1) {
                modCount += 1;
            }
            
            return modCount;

        });
        
        
        return modCount;
    };

    this.returnItemNameAndType = function() {
        var fullTitle = this.htmlDiv.find(".title").text().trim();
        for (var i = 0; i < this.items_types.length; ++i) {
            var currentItemType = this.items_types[i];
            if (fullTitle.indexOf(currentItemType) !== -1) {
                this.itemName = fullTitle.replace(currentItemType, "");
                this.itemType = currentItemType;
                return;
            }
        }

        this.itemName = fullTitle;
        this.itemType = 'Unknown';

    };

    this.returnItemSockets = function() {
        return this.htmlDiv.find(".sockets-raw").text().trim();
    };

    this.returnLevelRequirement = function() {
        return this.htmlDiv.find(".first-line li").first().text().replace("Level:", "").trim();
    };

    this.returnItemLevel = function() {
        return this.htmlDiv.find("span[data-name='ilvl']").text().replace("ilvl:", "").trim();
    };
    
    this.returnRarity = function() {
        var rarityIndex = this.htmlDiv.find(".title").attr("class").match(/[0-9]/);
        return this.item_rarities[Number(rarityIndex)];
    };

    this.returnImplicit = function() {
        var enchanted = this.returnEnchantedMods();
        
        if (enchanted !== "") {
            return enchanted.trim();
        } else {
            return this.returnImplicitMods();
        }
    };
    
    this.returnImplicitMods = function() {
        var itemMods = "";
        this.htmlDiv.find(".withline li").each(function(){
            var that = $(this);
            itemMods += that.text() + '\r\n';
        });
        return itemMods.trim();
    };
    
    this.returnEnchantedMods = function() {
        var itemMods = "";
        this.htmlDiv.find("ul .mods:not(.withline) li").each(function(){
            var that = $(this);
   
            if (that.text().indexOf('enchanted') !== -1) {
                itemMods += '{crafted}' + that.text().replace("enchanted", "").trim() + '\r\n';
            }

        });
        return itemMods;
    };

    this.returnCraftedMods = function() {
        var craftedItems = "";
        this.htmlDiv.find("ul .mods:not(.withline) li").each(function(){
            var that = $(this);
   
            if (that.text().indexOf('crafted') !== -1) {
                craftedItems += '{crafted}' + that.text().replace("crafted", "").trim() + '\r\n';
            }
        });
        return craftedItems;
    };

    
    this.returnItemMods = function() {
        var itemMods = "";
        this.htmlDiv.find("ul .mods:not(.withline) li").each(function(){
            var that = $(this);
            if (that.text().indexOf('enchanted') === -1 && 
                that.text().indexOf('crafted') === -1 &&
                that.text().indexOf('total') === -1 &&
                that.text().indexOf('pseudo') === -1) {
                itemMods += that.contents().not($("span")).text().trim() + '\r\n';
            }
        });
        return itemMods;
    };
     
}

PoeItem.prototype.buildItem = function() {

    var itemMods = this.returnItemMods();
    var implicit = this.returnImplicit();

    this.returnItemNameAndType();

    var item = "";
    
    var rarity = this.returnRarity();
    if (rarity !== "")
        item += "Rarity: " + rarity + '\r\n';
    
    item += this.itemName + '\r\n';
    item += this.itemType + '\r\n';
    item += 'Item Level: ' + this.returnItemLevel() + '\r\n';

    var quality = this.returnItemQuality();
    if (quality !== "")
        item += "Quality: " + quality + '\r\n';

    var sockets = this.returnItemSockets();
    if (sockets !== "")
        item += "Sockets: " + sockets + '\r\n';

    item += 'Implicits: ' + this.returnImplicitCount() + '\r\n';

    if (implicit !== "")
        item += implicit + '\r\n';

    item += itemMods;

    var crafted = this.returnCraftedMods();
    if (crafted !== "") {
        item += crafted;
    }

    return item.trim();

};
