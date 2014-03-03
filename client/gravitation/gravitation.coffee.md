	
graivation constant

	G = 1

	calculateAllPositions = (objects, dt) ->

because we do an equals in getAcceleration we can compare the object to itself
otherwise, we should leave out the object itself in the array passed to calculateNewPosition

		calculateNewPosition obj, objects, dt for obj in objects


here we ignore to objects that are considered equal in its position x. it would create floating point issues or divisions by zero

	getAcceleration = (obj, otherObjects) ->
		a_total = new Vector obj.x.data.length
		for otherObj in otherObjects
			unless obj.x.equals otherObj.x
				acc = obj.x.sub otherObj.x
				acc._multi -G * otherObj.m*(1/acc.length() ** 3)
				a_total._add acc
		return a_total


	
	calculateNewPosition = (obj, otherObjects, dt) ->

acceleration of an object is given by newtons gravitation law

		acc = getAcceleration obj, otherObjects

velocity is acceleration * dt
		
		obj.v._add acc._multi dt

and the position is veolicty * dt

		obj.x._add obj.v.multi dt




	Router.map -> @route 'gravitation'

	Template.gravitation.rendered = ->

		console.log @		

		TIME_STEP = 1


		sample1 = [
			(
				m: 1
				x: new Vector [200,200] 
				v: new Vector [0.1,0]
			)
			(
				m: 1
				x: new Vector [150,150]
				v: new Vector [0,0.1]
			)
			]



		sample2 = for i in [0..10]
			x = _.random 0,1000
			y = _.random 0,1000
			vx = 0
			vy = 0
			object = 
				m: 1
				x: new Vector [x,y]
				v: new Vector [vx,vy]
				
				
				

		canvas = @find "canvas"
		if canvas?
			context = canvas.getContext "2d"
			


		draw = (objects) ->


			offsetX = 0
			offsetY = 0

			context.fillStyle = "red"
			for obj in objects
			

				context.fillRect(offsetX + obj.x.data[0],offsetY + obj.x.data[1],1,1)


		run = (objects) ->
			step = ->
				calculateAllPositions objects, TIME_STEP
				draw objects
				_.delay step, 0
			step()

		


start it
		
		run sample1



	
