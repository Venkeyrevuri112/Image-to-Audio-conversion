if request.method == 'POST':
        img = request.files['file']
        language = request.form['language']
        print(language)

        if img:
            # Get the selected file name
            file_name = img.filename
            print("Uploaded File Name:", file_name)
            img.save('./static/assets/result.png')
            max_length, vocabs, inv_vocabs, model, resnet_model = load_model()
            idx, caps = get_captions(img)
            image = cv2.imread('static/assets/result.png')
            predicted_text = predict_caption(max_length, vocabs, inv_vocabs, model, image, resnet_model, idx, caps)
            predicted_text = predicted_text[1] if idx else predicted_text[0]
            print(predicted_text)
            speak_text(predicted_text, language)
            
            return jsonify({'audio': '../static/assets/result.mp3', 'caption':predicted_text})
        

    return render_template('video.html', lang=lang)


if __name__ == '__main__':
    app.run(debug=True)
