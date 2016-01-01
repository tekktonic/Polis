function Fenestral(){
    var maptiles;
    var thingy;
    var tileIdx = 0;

    this.setup = function()
    {
        function handleMap()
        {

        }
        var reqTmp = new XMLHttpRequest();

        reqTmp.open("GET", "map.json", false);
        reqTmp.send(null);
        var jsonMap = eval('(' + reqTmp.responseText + ')');
        this.tiles = new jaws.SpriteSheet({image: "tileset.png", frame_size: [jsonMap.tilewidth, jsonMap.tileheight], scale_image: 1, orientation: "right"});
        this.thingy = createObject({components: {position: new cPosition(0, 0), drawable: new cDrawable(this.tiles.frames[114]), hp: new cHp(100)}});
        thingy = this.thingy;

        var tiles = this.tiles;
        this.map = _.map(jsonMap.map, function(elm, idx, list)
                         {
                             return createObject({components: {position: new cPosition(Math.floor((idx / jsonMap.width)) * jsonMap.tilewidth, Math.floor((idx % jsonMap.width) * jsonMap.tileheight)),
                                                               drawable: new cDrawable(tiles.frames[(elm.tileY * jsonMap.width) + (elm.tileX)])}})
                         });
        
    };
    
    this.update = function()
    {
        currentTile = new jaws.Sprite({image: this.tiles.frames[tileIdx % this.tiles.frames.length],
                                       x: 64, y: 64});
        tileIdx += 1;
    };
    
    this.draw = function()
    {
        jaws.clear();
        _.each(this.map, function(elm, idx, l){elm.draw()});
        thingy.draw();
        currentTile.draw();
    };


    function createObject(o)
    {
        function componentDraw()
        {
            _.each(this.components, function(elm, idx, l)
                   { if (elm.draw) { elm.draw();} });
        };

        function componentClaim(o)
        {
            _.each(o.components, function(elm, idx, l)
                   { console.log("elm: " + elm); elm.owner = o });
        };
        
        var ret = o;

        componentClaim(ret);
        ret.draw = _.bind(componentDraw, ret);
        return ret;
    };
    
    function cPosition(x, y)
    {
        this.x = x;
        this.y = y;
    };

    function cDrawable(sprite)
    {
        this.sprite = new jaws.Sprite({image: sprite});

        this.draw = function()
        {
            this.sprite.x = this.owner.components.position.x;
            this.sprite.y = this.owner.components.position.y;
            this.sprite.draw();
        }
    };


    // Just a rock paper scissors for now. sword beats axe, axe beats spear, spear
    // beats sword.
    // Functions are just markers, maybe add stuff later
    var swordsman = 0;
    var spearman = 1;
    var axeman = 2;
    function cFighter(type)
    {
        
        this.type = type;
        
        function fight(other)
        {
            switch (this.type)
            {
                case swordsman:
                if (other.components.fighter.type == this.type
                    || other.components.fighter.type ==  axeman)
                    return this;
                else
                    return other;
                break;

                case axeman:
                if (other.components.fighter.type == axeman
                   || other.components.fighter.type == spearman)
                    return this;
                else
                    return other;
                break;
                
                case spearman:
                if (other.components.fighter.type == spearman
                   || other.components.fighter.type == swordsman)
                    return this;
                else
                    return other;
                break;
            }
        }
    }
    
    function cHp(maxHP){
        this.maxHP = maxHP;
        this.currentHP = maxHP;
        var rectangleWidth = 32;
       
        this.damage = function(damage){
            this.currentHP = this.currentHP - damage;
        }
       
        this.draw = function(){
        	var hpPercent = this.currentHP / maxHP;
            var drawWidth =  hpPercent * rectangleWidth;
            var ctx = jaws.context;
            
            if(hpPercent >= .75)
            	ctx.fillStyle="rgba(0, 255, 0, 0.75)";
            else if(hpPercent >= .50)
                ctx.fillStyle="rgba(255, 255, 0, 0.75)";
            else if(hpPercent >= .25)
                ctx.fillStyle="rgba(255, 200, 0, 0.75)";
            else
                ctx.fillStyle="rgba(255, 0, 0, 0.75)";
            
            ctx.fillRect(this.owner.components.position.x,
                         this.owner.components.position.y + 28,
                         drawWidth, 4);
           
            ctx.lineWidth="1";
            ctx.rect(this.owner.components.position.x, this.owner.components.position.y + 28,
                     rectangleWidth, 4);
            ctx.stroke();
        }
       
    };
}


