
usage: 
a = new Vector [1,2,5]
b = new Vector [3,-3,0.5]

c = a.add b  # will not change a or b
a._add b # will add b to a (see comment below)

d = a.multi 5




underscored methods will change the vector itself and return itself for chaning (the @ at the end of the function)
non-underscore functions will return a new copy

use underscored functions for better performance, as they do not initialize new objects on every calculation

	class @Vector

		constructor: (dimensionOrArray) ->
			if dimensionOrArray instanceof Array
				@data = dimensionOrArray
			else
				@data = Array.apply(null, Array(dimensionOrArray)).map -> 0
			
		add: (vector) ->
			@checkDim vector
			new Vector(value += vector.data[i] for value, i in @data)

		_add: (vector) ->
			@checkDim vector
			@data[i] += vector.data[i] for value, i in @data
			@ # return itself for chaining
				

		sub: (vector) ->
			@checkDim vector
			new Vector(value -= vector.data[i] for value, i in @data)

		_sub: (vector) ->
			@checkDim vector
			@data[i] -= vector.data[i] for value, i in @data
			@ # return itself for chaining

		multi: (scale) ->
			new Vector(value *= scale for value, i in @data)

		_multi: (scale) ->
			@data[i] *= scale for value, i in @data
			@ # return itself for chaining

		clone: ->
			new Vector @data

		length: ->
			sum = 0
			sum += value * value for value in @data
			Math.sqrt sum

		# skalar product
		dot: (vector) ->
			@checkDim vector
			sum = 0
			sum += value * vector.data[i] for value, i in @data
			sum

		checkDim: (vector) ->
			unless @hasSameDim vector
				throw new Error("dimensions do not match")

		hasSameDim: (vector) ->
			vector.data.length == @data.length

		equals: (vector, tolerance = 0) ->
			return false unless @hasSameDim vector
			for value, i in @data
				if Math.abs(value - vector.data[i]) > tolerance
					return false
			return true


		toString: -> "("+@data.toString()+")"

