


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
		context.fillStyle = "red"


		draw = (objects) ->
			for obj in objects
				context.fillRect(obj.x.data[0],obj.x.data[1],1,1)


		run = (objects) ->
			step = ->
				GravitationEngine.step objects, TIME_STEP, -> 
					draw objects
					_.delay step, 0
			step()

		


start it
		
		run sample1



	
