-----------------------------------------------------------------------------------------
-- Created by: Aayman Shameem
-- Created on: Apr 24, 2018
--
-- This code will help the character jump properly and not tilt :D
-----------------------------------------------------------------------------------------

-- Gravity

local physics = require( "physics" )

physics.start()
physics.setGravity( 0, 25 ) -- ( x, y )
physics.setDrawMode( "hybrid" )   -- Shows collision engine outlines only

local leftWall = display.newRect( 0, display.contentHeight / 2, 1, display.contentHeight )
-- myRectangle.strokeWidth = 3
-- myRectangle:setFillColor( 0.5 )
-- myRectangle:setStrokeColor( 1, 0, 0 )
leftWall.alpha = 0.0
physics.addBody( leftWall, "static", { 
    friction = 0.5, 
    bounce = 0.3 
    } )

local simpleDirt = display.newImage( "./assets/sprites/land.png" )
simpleDirt.x = display.contentCenterX - 510
simpleDirt.y = display.contentHeight
simpleDirt.id = "the ground"
physics.addBody( simpleDirt, "static", { 
    friction = 0.5, 
    bounce = 0.3 
    } )

local moreDirt = display.newImage( "./assets/sprites/land.png" )
moreDirt.x = display.contentCenterX + 800
moreDirt.y = display.contentCenterY + 100
moreDirt.id = "the ground"
physics.addBody( moreDirt, "static", { 
    friction = 0.5, 
    bounce = 0.3 
    } )


local monsterCat = display.newImage( "./assets/sprites/Monstercat.png" )
monsterCat.x = display.contentCenterX + 900
monsterCat.y = display.contentCenterY - 500
monsterCat.id = "the character"
physics.addBody( monsterCat, "dynamic", { 
    density = 3.0, 
    friction = 0.5, 
    bounce = 0.3 
    } )
monsterCat.isFixedRotation = true -- If you apply this property before the physics.addBody() command for the object, it will merely be treated as a property of the object like any other custom property and, in that case, it will not cause any physical change in terms of locking rotation.


local dPad = display.newImage( "./assets/sprites/d-pad.png" )
dPad.x = 150	
dPad.y = display.contentHeight - 150
dPad.id = "d-pad"
dPad.alpha = 0.5

local upArrow = display.newImage( "./assets/sprites/upArrow.png" )
upArrow.x = 150
upArrow.y = display.contentHeight - 260
upArrow.id = "up arrow"

local downArrow = display.newImage( "./assets/sprites/downArrow.png" )
downArrow.x = 150
downArrow.y = display.contentHeight - 40
downArrow.id = "down arrow"

local leftArrow = display.newImage( "./assets/sprites/leftArrow.png" )
leftArrow.x = 40
leftArrow.y = display.contentHeight - 150
leftArrow.id = "left arrow"

local rightArrow = display.newImage( "./assets/sprites/rightArrow.png" )
rightArrow.x = 260
rightArrow.y = display.contentHeight - 150
rightArrow.id = "right arrow"

local jumpButton = display.newImage( "./assets/sprites/jumpButton.png" )
jumpButton.x = display.contentWidth - 80
jumpButton.y = display.contentHeight - 80
jumpButton.id = "jump button"
jumpButton.alpha = 0.5
 
function upArrow:touch( event )
    if ( event.phase == "ended" ) then
        -- move the character up
        transition.moveBy( monsterCat, { 
        	x = 0, -- move 0 in the x direction 
        	y = -50, -- move up 50 pixels
        	time = 100 -- move in a 1/10 of a second
        	} )
    end

    return true
end

function downArrow:touch( event )
    if ( event.phase == "ended" ) then
        -- move the character up
        transition.moveBy( monsterCat, { 
        	x = 0, -- move 0 in the x direction 
        	y = 50, -- move up 50 pixels
        	time = 100 -- move in a 1/10 of a second
        	} )
    end

    return true
end

function leftArrow:touch( event )
    if ( event.phase == "ended" ) then
        -- move the character up
        transition.moveBy( monsterCat, { 
        	x = -50, -- move 0 in the x direction 
        	y = 0, -- move up 50 pixels
        	time = 100 -- move in a 1/10 of a second
        	} )
    end

    return true
end

function rightArrow:touch( event )
    if ( event.phase == "ended" ) then
        -- move the character up
        transition.moveBy( monsterCat, { 
        	x = 50, -- move 0 in the x direction 
        	y = 0, -- move up 50 pixels
        	time = 100 -- move in a 1/10 of a second
        	} )
    end

    return true
end

function jumpButton:touch( event )
    if ( event.phase == "ended" ) then
        -- make the character jump
        monsterCat:setLinearVelocity( 0, -750 )
    end

    return true
end

-- if character falls off the end of the world, respawn back to where it came from
function checkCharacterPosition( event )
    -- check every frame to see if character has fallen
    if monsterCat.y > display.contentHeight + 500 then
        monsterCat.x = display.contentCenterX - 200
        monsterCat.y = display.contentCenterY
    end
end

upArrow:addEventListener( "touch", upArrow )
downArrow:addEventListener( "touch", downArrow )
leftArrow:addEventListener( "touch", leftArrow )
rightArrow:addEventListener( "touch", rightArrow )
jumpButton:addEventListener( "touch", jumpButton )
Runtime:addEventListener( "enterFrame", checkCharacterPosition )
